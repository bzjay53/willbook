# Willbook 리팩토링 가이드라인

## 1. 코드 품질 기준
### 1.1 함수 설계 원칙
- 단일 책임 원칙 준수 (함수당 1개의 명확한 목적)
- 15라인 이내 함수 권장
- 명시적인 반환 타입 지정

```typescript
// Good
function calculateUserCredits(subscription: Subscription): number {
  return subscription.tier === 'premium' ? 1000 : 500;
}

// Bad 
function processUser(user) {
  // 여러 작업을 혼합
}
```

### 1.2 컴포넌트 설계
- Presentational/Container 패턴 적용
- 재사용 가능한 훅 분리
- Props 타입 명시적 정의

## 2. 리팩토링 프로세스
1. 테스트 커버리지 확인 (80%+ 목표)
2. 영향도 분석 수행
3. 작은 단위로 단계적 리팩토링
4. 기능 회귀 테스트

## 3. 주요 리팩토링 패턴
### 3.1 인증 시스템
- AuthProvider 컴포넌트 분리
- 커스텀 훅으로 로직 캡슐화
```typescript
// Before: 직접 Supabase 호출
// After: 훅 사용
const { user, login } = useAuth();
```

### 3.2 API 레이어
- API 클라이언트 단일화
- 에러 처리 표준화

## 4. 모니터링 지표
- 순환 복잡도 감소 추이
- 테스트 커버리지 변화
- 빌드 시간 개선 정도