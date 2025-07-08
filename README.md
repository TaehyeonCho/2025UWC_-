# UWC (Ubiquitous Welfare Care) 시스템

실시간 AI 기반 복약 관리 및 낙상 감지 시스템

## 주요 기능

- 실시간 식사 감지: AI로 식사 여부를 실시간 감지
- 복약 알림: 식사 후 30분 뒤 음성으로 복약 알림
- 낙상 감지: 3D CNN으로 실시간 낙상 감지 및 SMS 알림
- 휴지 감지: YOLOv8로 휴지 개수 실시간 카운트
- 쌀 잔량 측정: U-Net으로 쌀 잔량 퍼센트 측정
- 실시간 영상: 부드러운 실시간 카메라 스트림

## 대용량 파일 다운로드 안내

GitHub 용량 제한(25MB)으로 인해, src 폴더를 제외한 모든 파일은 아래 구글 드라이브에서 다운로드할 수 있습니다.

- [src 제외 전체 파일 다운로드(Google Drive)](https://drive.google.com/drive/folders/1R-wrMv4d6z9EbLdhnq0Y38pQVdfKeQs2?usp=sharing)

다운로드 후, 압축을 해제하여 프로젝트 루트에 배치해 주세요.

```
프로젝트/
├── runs/
├── models/
├── requirements.txt
├── README.md
└── ...
```

> 참고:  
> - data 폴더(학습용 데이터)는 서비스 실행에 필요하지 않으므로, 없어도 무방합니다.

## 시스템 요구사항

### 하드웨어
- **카메라**: USB 웹캠 또는 내장 카메라
- **RAM**: 최소 4GB (AI 모델 실행용)
- **GPU**: 선택사항 (CUDA 지원 시 더 빠름)

### 소프트웨어
- **Python**: 3.8 이상
- **MySQL**: 8.0 이상
- **운영체제**: Windows 10/11, Linux, macOS

## 설치 가이드

### 1. 저장소 클론
```bash
git clone <repository-url>
cd uwc-system
```

### 2. Python 가상환경 생성
```bash
# Windows
python -m venv uwc_env
uwc_env\Scripts\activate

# Linux/Mac
python3 -m venv uwc_env
source uwc_env/bin/activate
```

### 3. 패키지 설치
```bash
pip install -r requirements.txt
```

### 4. MySQL 설정
```sql
-- MySQL에 접속 후 실행
CREATE DATABASE uwc_db;
USE uwc_db;

-- 테이블 생성
CREATE TABLE item_count (
    id INT AUTO_INCREMENT PRIMARY KEY,
    item_name VARCHAR(50) NOT NULL,
    count FLOAT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 5. 환경 설정
`src/db.py`와 `src/main.py`에서 MySQL 연결 정보를 수정하세요:
```python
conn = mysql.connector.connect(
    host='localhost',      # MySQL 서버 주소
    user='your_username',  # MySQL 사용자명
    password='your_password', # MySQL 비밀번호
    database='uwc_db'      # 데이터베이스명
)
```

## 사용법

### 1. 프로그램 실행
```bash
python src/main.py
```

### 2. 기능 사용
- **실시간 영상**: "Camera" 창에서 실시간 영상 확인
- **종료**: 카메라 창에서 **q 키** 누르기
- **감지 결과**: 콘솔에서 실시간 감지 결과 확인

### 3. 감지 주기
- **식사/쌀/휴지 감지**: 1초마다
- **낙상 감지**: 5초마다
- **DB 저장**: 5초마다

## 프로젝트 구조

```
uwc-system/
├── src/
│   ├── main.py              # 메인 프로그램
│   ├── db.py                # 데이터베이스 연결
│   ├── mealDetect/          # 식사 감지 모델
│   ├── pillDetect/          # 복약 감지 모델
│   ├── fallDetect/          # 낙상 감지 모델
│   ├── toiletpaperDetect/   # 휴지 감지 모델
│   └── riceDetect/          # 쌀 감지 모델
├── models/
│   └── riceSeg.pt           # 쌀 감지 모델 파일
├── runs/
│   └── detect/              # YOLO 모델 파일들
├── data/                    # 데이터셋
├── requirements.txt          # 패키지 목록
└── README.md               # 이 파일
```

## 🔧 문제 해결

### 카메라 오류
```bash
# 카메라 권한 확인 (Linux)
sudo usermod -a -G video $USER
```

### MySQL 연결 오류
```bash
# MySQL 서비스 시작
# Windows
net start mysql

# Linux
sudo systemctl start mysql
```

### CUDA 오류 (GPU 사용 시)
```bash
# CPU 버전으로 설치
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
```

## 지원

문제가 발생하면 다음을 확인하세요:
1. Python 버전 (3.8 이상)
2. MySQL 서버 실행 상태
3. 카메라 연결 상태
4. 패키지 설치 완료 여부

## 라이선스

이 프로젝트는 교육 및 연구 목적으로 제작되었습니다. 