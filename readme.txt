.
├── src/
│   ├── config/         # 환경 설정 관련 파일 (예: db.js, env.js)
│   ├── controllers/    # 요청 처리 및 응답 생성 (request/response 핸들링)
│   ├── services/       # 비즈니스 로직 구현
│   ├── repositories/   # 데이터베이스 접근 로직 (Supabase 클라이언트 사용)
│   ├── routes/         # API 엔드포인트 및 컨트롤러 연결
│   ├── utils/          # 공통 유틸리티 함수
│   └── app.js          # 애플리케이션 진입점 (Express 앱 설정 등)
├── tests/              # 테스트 코드
├── .env.example        # 환경 변수 예시 파일 (민감 정보 제외)
├── Dockerfile          # Docker 이미지 빌드 정의
├── docker-compose.yml  # Docker Compose 설정
├── package.json        # Node.js 프로젝트 설정 및 의존성 관리
└── README.md
