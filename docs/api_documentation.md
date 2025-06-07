# API 문서화 가이드

## 1. 기술 스택
```bash
# 의존성 설치
npm install swagger-ui-express swagger-jsdoc yamljs
```

## 2. 기본 설정
```ts
// swaggerConfig.ts
import swaggerJsdoc from 'swagger-jsdoc';

const options = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'Willbook API',
      version: '1.0.0',
      description: '도서 출판 플랫폼 API 문서'
    },
    servers: [
      { url: 'https://api.willbook.com/v1' }
    ]
  },
  apis: ['./src/routes/*.ts'] // API 경로 패턴
};

export const specs = swaggerJsdoc(options);
```

## 3. 엔드포인트 문서화 예시
```ts
/**
 * @swagger
 * /books:
 *   get:
 *     summary: 도서 목록 조회
 *     tags: [Books]
 *     parameters:
 *       - in: query
 *         name: limit
 *         schema:
 *           type: integer
 *           default: 10
 *     responses:
 *       200:
 *         description: 도서 목록
 *         content:
 *           application/json:
 *             schema:
 *               type: array
 *               items:
 *                 $ref: '#/components/schemas/Book'
 */
router.get('/books', bookController.getBooks);
```

## 4. 데이터 모델 정의
```ts
/**
 * @swagger
 * components:
 *   schemas:
 *     Book:
 *       type: object
 *       properties:
 *         id:
 *           type: string
 *           format: uuid
 *         title:
 *           type: string
 *           example: "새로운 출판의 시대"
 *         author:
 *           type: string
 *           example: "홍길동"
 */
```

## 5. 에러 응답 표준
```ts
/**
 * @swagger
 * components:
 *   responses:
 *     UnauthorizedError:
 *       description: 인증 실패
 *       content:
 *         application/json:
 *           schema:
 *             $ref: '#/components/schemas/Error'
 * 
 *   schemas:
 *     Error:
 *       type: object
 *       properties:
 *         code:
 *           type: string
 *           example: "AUTH_001"
 *         message:
 *           type: string
 *           example: "유효하지 않은 토큰"
 */
```

## 6. 로컬 실행 방법
```bash
# API 문서 서버 실행
npm run serve:docs

# 접속 URL
http://localhost:3000/api-docs
```

## 7. 문서 버전 관리
```yaml
# .swagger-config.yaml
versioning:
  current: 1.0.0
  deprecated:
    - 0.9.0
    - 0.8.2