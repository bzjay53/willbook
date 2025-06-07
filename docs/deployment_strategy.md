# 배포 전략 문서

## 1. 인프라 아키텍처
```mermaid
graph TD
    A[CloudFront CDN] --> B[API Gateway]
    A --> C[S3 정적 호스팅]
    B --> D[Lambda 함수]
    D --> E[RDS PostgreSQL]
    D --> F[ElastiCache Redis]
```

## 2. CI/CD 파이프라인
```yaml
# .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Install dependencies
        run: npm ci
        
      - name: Run tests
        run: npm test
        
      - name: Build production
        run: npm run build
        
      - name: Deploy to AWS
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_KEY }}
          aws-region: ap-northeast-2
        run: |
          npm run deploy:prod
```

## 3. 배포 단계
1. **Canary 배포**:
   - 10% 트래픽 신버전으로 라우팅
   - 30분 모니터링 후 문제 없으면 점진적 확대

2. **롤백 절차**:
```bash
# 이전 버전으로 롤백
aws lambda update-alias --function-name willbook-api \
  --name production --function-version $PREVIOUS_VERSION
```

## 4. 모니터링 설정
```json
// cloudwatch-alarms.json
{
  "Api5xxErrors": {
    "MetricName": "5XXError",
    "Threshold": 1,
    "EvaluationPeriods": 5,
    "AlarmActions": ["arn:aws:sns:ap-northeast-2:1234567890:alarm-topic"]
  },
  "HighLatency": {
    "MetricName": "Latency",
    "Threshold": 1000,
    "EvaluationPeriods": 3
  }
}
```

## 5. 환경 변수 관리
```bash
# AWS Systems Manager Parameter Store
aws ssm put-parameter \
  --name "/willbook/prod/DB_URL" \
  --value "postgres://user:pass@host:5432/db" \
  --type SecureString
```

## 6. 용량 계획
```table
| 리소스         | 개발      | 스테이징 | 프로덕션 |
|----------------|-----------|-----------|----------|
| RDS 인스턴스   | t3.micro  | t3.small  | t3.large |
| Lambda 메모리  | 512MB     | 1024MB    | 2048MB   |
| Cache 노드     | cache.t3  | cache.t3  | cache.m6 |
```

## 7. 재해 복구 절차
1. **RDS 스냅샷 복원**:
```bash
aws rds restore-db-instance-from-db-snapshot \
  --db-instance-identifier willbook-dr \
  --db-snapshot-identifier willbook-latest-snapshot
```

2. **트래픽 전환**:
```bash
aws route53 change-resource-record-sets \
  --hosted-zone-id Z123456789 \
  --change-batch file://dns-switch.json