---
name: image-nanobanana
description: Gemini NanoBanana 이미지 생성 — AI 이미지 생성 (Ghibli, Educational 스타일)
user-invocable: false
paths: short-video-maker/src/image-generation/**
---

## 용도
Gemini NanoBanana 모델로 AI 이미지 생성. Books/Poetry/News 프로젝트 씬 이미지에 사용.
9:16 세로 비율, 스타일 프리픽스 자동 적용, 캐릭터 레퍼런스 모드 지원.

## 핵심 파일
- `src/image-generation/services/ImageGenerationService.ts` — 메인 클래스
- `src/image-generation/types.ts` — ImageModelType, ImageGenerationResult 타입
- `src/image-generation/providers/` — 모델별 프로바이더 (NanoBanana, GPT-4o 등)

## 의존성
- `@google/generative-ai` — Gemini SDK
- 환경변수: `GOOGLE_GEMINI_API_KEY`

## API/인터페이스
```typescript
class ImageGenerationService {
  async generateImages(
    options: {
      prompt: string;
      numberOfImages: number;
      aspectRatio: string; // "9:16" for portrait
    },
    videoId: string,
    sceneIndex: number
  ): Promise<{ success: boolean; images: Array<{ data: Buffer }> }>;
}

enum ImageModelType {
  NANO_BANANA = "nano_banana",
  // GPT_4O 코드 보존되어 있으나 미사용
}
```

## 다른 프로젝트에 이식하기
1. `src/image-generation/` 폴더 전체 복사
2. `npm install @google/generative-ai`
3. 환경변수 `GOOGLE_GEMINI_API_KEY` 설정
4. `ImageGenerationService` 인스턴스 생성 후 `generateImages()` 호출

## 주의사항
- GPT-4o 프로바이더 코드가 보존되어 있으나 현재 NanoBanana만 사용
- 스타일별 프롬프트 프리픽스가 자동 적용됨 (Ghibli, Educational 등)
- 이미지당 API 비용 발생 — numberOfImages 최소화 권장
- 9:16 세로 비율이 기본값 (Shorts용)
