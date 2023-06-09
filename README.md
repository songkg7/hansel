# Hansel

간식 신청을 편리하게!

## 요구사항

- `/snack` 이라는 명령을 통해 사용자에게만 보이는 특정 입력 폼을 노출하고, submit 버튼을 통해 스프레드시트를 업데이트
- `/status` 명령을 통해 현재 신청된 간식 목록 가져오기
- 매달 정해진 날짜에 공지 채널을 통해 멤버들에게 알림 주기
- Optional. 즐겨찾는 간식 목록 제공
- Bot 설정을 관리자가 수정 가능하게
  - google speadsheet url
  - customize start range
- 구매 상태 변경시 신청한 사람에게 DM 발송하기
  - empty -> complete only

## 개략적 설계

- 월간 사용자 30 남짓
- API 요청이 많지 않을 것

discord 명령어 `/snack` 을 통해 간식 링크 및 description 을 구글 스프레드시트에 삽입해주는 API

| 날짜         | 이름   | 품명  | 링크 | 구매여부 |
|------------|------|-----|----|------|
| 2023-05-21 | Tom  | 꼬북칩 |    | 대기 중 |
| 2023-05-21 | Mike | 콘칩  |    | 대기 중 |

AWS Lambda 를 사용하면 별도의 서버 없이도 간식 API 를 제공 가능

자주 실행될 API 가 아니기 때문에 서버를 24시간 유지할 필요가 없으므로, Lambda 를 통해 간략하게 구성하면 비용 없이 API 를 호출할 수 있다.

```mermaid
sequenceDiagram
    Discord ->>+ AWS Lambda: 간식 신청
    AWS Lambda ->> Google SpreadSheet: 입력
    AWS Lambda -->>- Discord: Great!
```
