### Context
GCP VM에 n8n 셀프호스팅을 구축하면서 DuckDNS를 사용하여 Let's Encrypt로 HTTPS 인증서를 발급받으려 했습니다. 하지만 DuckDNS 도메인을 사용할 때 CAA 레코드 조회 오류로 인해 계속 실패하는 문제가 발생했습니다.

### Core
Let's Encrypt가 DuckDNS의 CAA 레코드를 조회할 때 DNS 문제로 SERVFAIL 오류가 발생했습니다. 이 문제는 DuckDNS의 네임서버에서 간헐적으로 발생하는 것으로, CAA 레코드를 올바르게 조회하지 못하는 경우가 있습니다. 

해결책으로 Caddy를 통해 인증서를 발급받는 방법이 아닌, Ngrok을 사용하여 이 문제를 해결했습니다. Ngrok은 도메인, DNS, SSL 문제를 한 번에 해결해 주며, GCP VM의 방화벽 설정도 불필요하게 만듭니다.

### Insight
DuckDNS는 무료 DDNS 서비스라 안정적인 네임서버 운용이 어렵습니다. 개인 테스트 환경에서는 적합할 수 있지만, 안정성이 중요한 상용 서비스에는 부적절합니다. 대신 Ngrok을 사용하면 도메인 및 인증서 문제를 쉽게 해결할 수 있어 초기 셀프호스팅 설정에 유리합니다.

**출처:** [Let's Encrypt DNS 문제 해결](URL), [Ngrok 및 Caddy 비교](URL)