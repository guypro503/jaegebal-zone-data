# jaegebal-zone-data

재개발닷컴(jaegebal.com)에 등록된 정비구역 경계를 실제 지번 주소 목록으로 변환한 데이터.
탱크옥션(tankauction.com) 유저스크립트가 이 저장소의 `zones.json`을 불러와서 경매 물건 주소와 매칭한다.

## 파이프라인

1. jaegebal.com 구역 상세페이지 -> 관련 정부 고시(지형도면 고시 등) 탐색
2. 고시 PDF 다운로드 -> 첨부된 결정도(구역계) 지도 이미지 추출
3. 이미지에서 빨간 경계선(구역계)을 색상 분석으로 추출
4. 고시문에 명시된 공식 면적과 픽셀 면적을 비교해 축척(m/px) 자동 보정, jaegebal의 구역 중심좌표로 위치 보정
5. VWorld 공간정보 API로 폴리곤과 겹침비율 50% 이상인 지번만 채택
6. `zones.json`에 구역명/유형/색상/지번목록 저장

## zones.json 구조

```json
{
  "updated_at": "YYYY-MM-DD",
  "zones": [
    {
      "develop_id": 1952,
      "name": "모아성현동 1021",
      "type": "jaegebal",
      "detail_type": "moatown",
      "detail_type_display_name": "모아타운",
      "color": "#16a34a",
      "addresses": ["봉천동 1021", "봉천동 41-82", "..."]
    }
  ]
}
```

## 갱신 주기

주 1회 재실행 목표 (jaegebal.com 구역 정보 변경 추적).
