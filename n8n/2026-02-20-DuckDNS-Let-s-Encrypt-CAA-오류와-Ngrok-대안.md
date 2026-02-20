### Context
DuckDNS와 Let's Encrypt를 사용하여 GCP VM에 n8n의 셀프호스팅 환경을 구축하고자 하였습니다. Caddy를 통해 HTTPS 인증서를 발급 받으려 했으나, DuckDNS 도메인(`chutzrit.duckdns.org`)으로 인증서 발급이 지속적으로 실패하는 문제가 발생했습니다.

### Core
오류 메시지는 다음과 같습니다:
```
DNS problem: SERVFAIL looking up CAA for duckdns.org
- the domain's nameservers may be malfunctioning
```
Let's Encrypt가 인증서를 발급할 때 `duckdns.org`의 CAA 레코드를 조회하며, 이 과정에서 DuckDNS 네임서버가 SERVFAIL 오류를 반환하고 있었습니다. ZeroSSL로 CA를 변경하더라도 문제는 동일했습니다. 일부 사용자들은 VM의 DNS를 `1.1.1.1`로 변경하여 해결을 시도했지만 근본적인 해결책은 아니었습니다.

이에 결국 Caddy에서 Ngrok으로 전략을 변경하여 문제를 해결했습니다. Ngrok은 도메인/DNS/SSL 문제를 한번에 해결해주며, VM 방화벽 설정도 필요 없도록 지원합니다.

### Insight
DuckDNS는 무료 DDNS 서비스로 네임서버의 안정성이 보장되지 않기 때문에, 개인 테스트 용도로는 적합할 지 몰라도 안정적인 서비스가 필요한 환경에는 적합하지 않습니다. 반면, Ngrok은 손쉽게 설정 가능하고 재현성이 높은 환경을 제공하여 셀프호스팅 입문자에게 더 적합한 솔루션입니다.

**출처:** [Dns error issue](https://www.example.com), [Ngrok vs DuckDNS](https://www.example2.com)