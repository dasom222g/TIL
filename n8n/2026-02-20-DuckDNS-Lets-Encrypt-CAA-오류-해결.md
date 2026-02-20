### Context
Google Cloud Platform(GCP) VM에 n8n 셀프호스팅 환경을 구축하면서 Caddy 서버를 사용해 HTTPS 인증서를 발급받으려고 했습니다. DuckDNS 도메인(`chutzrit.duckdns.org`)을 통해 Let's Encrypt를 이용한 인증서 발급이 실패했습니다.

### Core
인증서 발급 오류 내용은 다음과 같습니다:
```
DNS problem: SERVFAIL looking up CAA for duckdns.org
- the domain's nameservers may be malfunctioning
```
Let's Encrypt는 인증서 발급 전 `duckdns.org`의 CAA 레코드를 조회하는데, 주기적으로 SERVFAIL 오류가 발생했습니다. 이는 ZeroSSL을 사용해도 동일하게 나타났습니다. VM의 DNS 설정을 `1.1.1.1`로 변경하여 문제를 해결할 수 있었지만, 이는 근본적인 해결책은 아니었습니다.

결국 **Ngrok**을 이용한 전략으로 변경하였습니다. Ngrok은 도메인, DNS, SSL 문제를 한번에 해결해주고, VM 방화벽 설정도 필요하지 않아 셀프호스팅 입문용으로 더 적합합니다.

### Insight
DuckDNS는 무료 DDNS 서비스로, 그만큼 네임서버의 안정성이 떨어질 수밖에 없습니다. 개인 테스트 환경에서는 적합할 수 있지만, 안정성과 재현성이 중요한 환경에서는 부적합한 선택임을 확인했습니다. Ngrok은 쉽게 사용 가능하고 많은 설정 문제를 피할 수 있어 초기 셀프호스팅 사용자에게 추천할 만합니다.

**출처:** [DuckDNS와 Let's Encrypt SERVFAIL 문제 해결](https://community.letsencrypt.org/t/servfail-when-querying-caa-for-duckdns-org/131824)