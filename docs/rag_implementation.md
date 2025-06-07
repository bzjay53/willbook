# RAG (Retrieval-Augmented Generation) 구현 가이드

## 1. 개요
- Willbook의 AI 생성 콘텐츠 품질 향상을 위한 RAG 아키텍처
- Supabase 벡터 DB와 OpenAI 임베딩 모델 연동

## 2. 핵심 컴포넌트
### 2.1 지식 베이스
- 구조화된 콘텐츠 저장을 위한 Supabase 테이블 설계
```sql
CREATE TABLE knowledge_base (
  id UUID PRIMARY KEY,
  content TEXT NOT NULL,
  embedding VECTOR(1536),
  metadata JSONB
);
```

### 2.2 검색 시스템
- 사용자 쿼리 임베딩 변환
- 코사인 유사도 기반 유사 문서 검색
```typescript
async function retrieveRelevantDocs(query: string, k: number = 3) {
  const embedding = await generateEmbedding(query);
  const { data } = await supabase.rpc('match_documents', {
    query_embedding: embedding,
    match_threshold: 0.7,
    match_count: k
  });
  return data;
}
```

## 3. 구현 단계
1. [ ] Supabase 벡터 확장 활성화
2. [ ] 임베딩 생성 API 엔드포인트 구현
3. [ ] 검색 결과 필터링 로직 추가
4. [ ] 생성 모델 프롬프트에 컨텍스트 주입

## 4. 모니터링 지표
- 검색 정확도 (Recall@K)
- 생성 응답 시간
- 사용자 피드백 점수