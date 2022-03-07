# Cookie vs Session

## HTTP의 특징

1. 클라이언트 - 서버
2. HTTP 메시지 (req, res)
3. stateless

## Stateless protocol

서버가 클라이언트의 이전 상태를 보존하지 않음.

만약, 사용자마다 다른 서비스를 제공하기 위해서는? 매번 사용자를 인증하기 위해 아이디와 비밀번호를 입력받고 전송해야함.

Cookie와 Session은 stateful한 서비스를 제공하기 위해 고안됨.

## Cookie vs Session

| 비교      | Cookie                             | Session                           |
| --------- | ---------------------------------- | --------------------------------- |
| 저장 장소 | client                             | server                            |
| 크기      | 제한됨(4KB)                        | 제한없음(서버의 능력에 따라 다름) |
| 의존성    | session에 의존적이지 않음          | cookie에 의존적임                 |
| 보안      | text file에 저장되고 안전하지 않음 | 쿠키에 비해 안전함                |

표로 비교 하고 있지만, 반대 개념이라고 볼 수는 없다.
심지어 session은 cookie를 사용해서 유지함.
