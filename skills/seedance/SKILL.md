---
name: image-seedance
description: Seedance 1.5 이미지-to-비디오 — Atlas Cloud 경유 $0.11/clip 영상 생성
user-invocable: false
paths: short-video-maker/src/YTB-seedance-server/**
---

## 용도
정지 이미지를 짧은 영상 클립으로 변환 (image-to-video). Poetry Shorts, Books 프로젝트에서
씬 이미지 → 모션 클립 생성에 사용. Seedance 1.5 Fast 모델, Atlas Cloud API 경유.

## 핵심 파일
- `src/YTB-seedance-server/src/atlas-cloud.ts` — AtlasCloudClient 메인 클래스
- `src/YTB-seedance-server/src/types.ts` — 요청/응답 타입 정의

## 의존성
- `axios` — HTTP 클라이언트
- 환경변수: `ATLAS_CLOUD_API_KEY`

## API/인터페이스
```typescript
class AtlasCloudClient {
  async generateVideo(options: {
    prompt: string;        // 영상 모션 설명
    image: string;         // base64 또는 URL
    duration: number;      // 초 단위 (5~10)
    orientation: string;   // "portrait" (9:16)
    cameraFixed: boolean;  // true 권장 (할루시네이션 방지)
  }): Promise<{ url: string }>;
}
```
- 모델: `bytedance/seedance-v1.5-pro/image-to-video-fast`
- 비용: ~$0.11/clip (최저가)

## 다른 프로젝트에 이식하기
1. `atlas-cloud.ts` + `types.ts` 복사
2. `npm install axios`
3. 환경변수 `ATLAS_CLOUD_API_KEY` 설정 (Atlas Cloud 계정 필요)
4. `new AtlasCloudClient(apiKey)` → `generateVideo()` 호출

## 주의사항
- **cameraFixed: true 필수** — false 시 Seedance가 할루시네이션 생성 (Critical Rule)
- 생성 완료까지 30초~2분 소요 (폴링 필요)
- portrait(9:16) 전용 — landscape 미지원
- 원본 이미지 품질이 결과에 직접 영향
- vast.ai 대비 Atlas Cloud가 가성비 우수 (검증 완료)
