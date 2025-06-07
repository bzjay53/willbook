# 디자인 시스템 문서

## 1. 색상 팔레트
### 기본 테마
```ts
const colors = {
  primary: {
    50: '#f0f9ff',
    100: '#e0f2fe',
    500: '#3b82f6', // 주요 액션 버튼
    600: '#2563eb' // 호버 상태
  },
  secondary: '#6b7280',
  success: '#10b981',
  error: '#ef4444'
}
```

### 다크 모드 대응
```ts
const darkColors = {
  background: '#1a202c',
  text: {
    primary: '#f7fafc',
    secondary: '#e2e8f0'
  }
}
```

## 2. 타이포그래피
### 폰트 스케일
```ts
const fontSizes = {
  xs: '0.75rem',
  sm: '0.875rem',
  base: '1rem',
  lg: '1.125rem',
  xl: '1.25rem',
  '2xl': '1.5rem'
}
```

### 타이포그래피 페어링
| 요소 | 폰트 크기 | 두께 | 행간 |
|------|----------|------|------|
| 제목 | 2xl | 700 | 1.2 |
| 본문 | base | 400 | 1.5 |
| 버튼 | sm | 600 | 1.25 |

## 3. 컴포넌트 라이브러리
### 버튼 변형
```tsx
<Button 
  colorScheme="primary"
  size="md"
  variant="solid|outline|ghost"
  isLoading={false}
/>
```

### 폼 컨트롤
```tsx
<FormControl isInvalid={!!error}>
  <FormLabel>책 제목</FormLabel>
  <Input type="text" />
  <FormErrorMessage>{error}</FormErrorMessage>
</FormControl>
```

## 4. 레이아웃 가이드
### 그리드 시스템
```tsx
<Grid
  templateColumns="repeat(auto-fill, minmax(300px, 1fr))"
  gap={6}
>
  {books.map(book => (
    <BookCard key={book.id} {...book} />
  ))}
</Grid>
```

### 반응형 브레이크포인트
```ts
const breakpoints = {
  sm: '30em', // 480px
  md: '48em', // 768px
  lg: '62em', // 992px
  xl: '80em'  // 1280px
}
```

## 5. 디자인 토큰
```json
{
  "spacing": {
    "1": "0.25rem",
    "2": "0.5rem",
    "4": "1rem"
  },
  "shadows": {
    "md": "0 4px 6px -1px rgba(0,0,0,0.1)"
  }
}