# 🎮 Unreal MCP Persona Project

<img src="MCP.jpg" width="450" height="800" style="aspect-ratio: 9/16; object-fit: cover;" alt="Verification">

> **"AI와 게임 엔진을 연결해 제작 파이프라인을 자동화하다."**  
>
> 본 프로젝트는 MCP(Claude) – Blender – Unreal Engine을 연동하여 게임 Persona3의 한 장면을 재현했습니다. 제작 파이프라인을 구축하고 자동화 흐름을 연구한 프로젝트입니다.

---

## 📋 1. 프로젝트 개요 (Overview)

* **프로젝트명:** Unreal MCP Persona 
* **유형:** AI 연동 게임 제작 파이프라인 연구 프로젝트 
* **개발 인원:** 1인 개발  
* **개발 목적:**
  - MCP(Claude)를 활용한 제작 자동화 실험
  - Blender → Unreal Engine 에셋 연동 구조 이해
  - AI 기반 콘텐츠 제작 워크플로우 설계
* **주요 특징:**  
  - Claude(MCP)를 활용한 설계 보조 자동화
  - Blender 모델링 → Unreal 자동 임포트 파이프라인 구축
  - Persona 콘셉트 기반 레벨 구성
  - 반복 작업 자동화를 통한 제작 속도 향상
---
## 🎥 2. 프로젝트 영상 (Demo Video)

> *아래 링크를 클릭하면 유튜브에서 고화질로 시청할 수 있습니다. (YouTube)*
> *특히나 이번 영상은 깃헙 업로드를 위해 압축을 많이 진행했습니다. 이에 음질과 화질 품질이 매우 낮은 점 양해바랍니다.*

[YouTube : MCP Persona 실습 영상](https://youtu.be/kqBEaP85d0M?si=sfnrLVSdC0KkfLN4)


https://github.com/user-attachments/assets/388e9769-66ab-4b57-a38c-5626b23aebde


---

## 🛠️ 3. 사용 기술 (Tech Stack)

### Engine & Language
*   **Unreal Engine 5.5**: Core Engine (최신 기능 활용)
*   **Blueprints**: 파티클 전용 머티리얼 제작 / Niagara Spawn 설정

### Modeling
*   **Blender**: Mesh 수정

### Tools
*   **Claude MCP**: Prompt 기반 Blender Mesh 초안 생성
*   **Nano banana**: Prompt 기반 Material Texture 생성

---

## 🔗 MCP – Blender – Unreal 파이프라인 구축 기록

### 📌 사용 환경
- Claude (MCP 서버 연동)
- Blender 3
- unreal-blender-mcp 브릿지
- Python 3.10+
- uv 패키지 매니저
---

### 🧠 1. MCP – Blender 연결 구축

### 1️⃣ MCP 서버 설치 및 실행

Claude가 Blender를 제어할 수 있도록 MCP 서버를 먼저 구축했다.

```bash
git clone https://github.com/tahooki/unreal-blender-mcp.git
cd unreal-blender-mcp
git submodule update --init --recursive

uv venv
source venv/bin/activate   # windows: venv\Scripts\activate
uv pip install -e .
```

서버 실행:
'''
uvx unreal-blender-mcp
'''

### 2️⃣ Claude 설정 파일 수정

Claude 설정 파일(.mcp.json)에 서버를 추가했다.

```
{
  "mcpServers": {
    "unreal-blender": {
      "command": "uvx",
      "args": ["unreal-blender-mcp"]
    }
  }
}
```

이 설정 이후 Claude가 Blender를 직접 제어할 수 있게 되었다.

### 3️⃣ Blender Add-on 설치

  1. Edit → Preferences → Add-ons
  2. Install 클릭
  3. blender-mcp/addon.py 선택
  4. 활성화 체크
  5. 3D View에서 N 패널을 열면 BlenderMCP 탭이 생성된다.
  6. Start Server 버튼을 눌러 연결을 시작했다.

### 4️⃣ MCP 활용 방식

Claude에게 다음과 같은 작업을 요청하며 제작을 진행했다.
  - 오브젝트 자동 생성
  - Modifier 자동 적용
  - 반복 구조 자동 배치
  - 카메라 / 라이트 세팅

복잡한 반복 모델링 작업 시간을 크게 단축했다.

### 🎨 2. Nano Banana 기반 Material 제작

Blender 노드 기반 쉐이더는 Unreal과 완전 호환되지 않기 때문에
텍스처 기반 PBR 워크플로우로 전환했다.

## 📌 Nano Banana MCP 연결

```
uv pip install nanobanana-pro-mcp
```
Claude 설정에 추가:
```
{
  "mcpServers": {
    "nanobanana": {
      "command": "uvx",
      "args": ["nanobanana-pro-mcp", "--api-key=MY_KEY". "--output-dir=./Content/Textures"]
    }
  }
}
```

### 1️⃣ 작업 방식
Nano Banana를 통해 다음 텍스처를 생성했다:
  - BaseColor
  - Normal
  - Roughness
  - Metallic
생성된 텍스처는 프로젝트 폴더에 자동 저장되도록 설정했다.
예시 프롬프트 :
```
4096x4096 PBR stone wall texture 생성해줘.
BaseColor, Normal, Roughness 각각 파일로 저장하고
Unreal 프로젝트 Content/Textures/Stone 폴더에 저장해줘.
```
Blender에서는 복잡한 노드 대신 텍스처 기반 PBR 구성으로 정리했다.

### 🔄 3. Blender → Unreal 파이프라인
에드온 및 플러그인을 활용해서 Blender에서 Mesh 수정 시 바로 Unreal에서 변화를 확인할 수 있도록 연결

### Blender 에드온 설정
  1. Add-ons **send2ue.zip**을 다운받은 후 Blender에 적용
  2. 생성된 Collection에 원하는 Mesh 이동
  3. Pipeline>Export>Send to Unreal로 Export 실행

+ Unreal 스케일 문제 방지를 위해 다음을 적용했다.
  - Unit Scale → 0.01
  - Forward → -Y
  - Up → Z
  - 모든 오브젝트 Ctrl + A → Transform Apply

### Unreal 설정
  - **Editor Scripting Utilities**, **Python Editor Script Plugin** 설치 확인
  - Project Settings에서 **Enable Remote Execution** true 확인

### 🧩 전체 제작 흐름

```
Claude (MCP)
   ↓
Blender 자동 모델링
   ↓
Nano Banana 텍스처 생성
   ↓
Blender PBR 구성
   ↓
Addon Export
   ↓
Plugin 자동 Import
   ↓
Material 재구성 및 레벨 배치
```

---


## 🚀 5. 트러블 슈팅 (Troubleshooting)
### 이슈 1: Blender → Unreal 스케일 불일치
**문제**: 메시 크기가 100배 차이 발생
**해결**: Blender Unit Scale 0.01 설정 후 Transform Apply



### 이슈 3: Niagara 파라미터 Blueprint 연동 실패
**문제**: 런타임 중 파라미터 값 변경 미적용
**해결**: User Parameter 사용 및 Set Niagara Variable 노드 적용


---

## 📚 6. 공부 확장 방향(Future Study Plan)
* GPU Particle Simulation 심화 학습  
* 이벤트(Event Handler) 기반 파티클 시스템 구조 이해  
* Collision 및 Physics 연동 심화 학습  
* Blueprint 및 C++과 Niagara 간 데이터 바인딩 구조 학습  
* 실제 게임 이펙트(폭발, 스킬 이펙트, 환경 효과) 분석 및 재구현  
* 최적화 전략 및 퍼포먼스 프로파일링 학습  

---

**Contact:** (강원우 / king_wonwoo@naver.com)  

