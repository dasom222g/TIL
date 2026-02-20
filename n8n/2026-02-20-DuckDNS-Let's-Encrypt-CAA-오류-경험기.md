### Context
GCP VM에 n8n 셀프호스팅 환경을 구축하면서 Caddy를 사용하여 HTTPS 인증서를 발급받고자 했습니다. DuckDNS 도메인(`chutzrit.duckdns.org`)으로 Let's Encrypt를 통해 인증서를 발급받으려 했으나 지속적으로 실패하는 문제가 발생했습니다.

### Core
실패 원인으로는 Let's Encrypt가 인증서 발급 시 `duckdns.org`의 CAA 레코드를 조회하려는데, DuckDNS의 네임서버가 간헐적으로 SERVFAIL 오류를 반환하는 것이었습니다. `1.1.1.1`과 같은 공개 DNS로 VM의 DNS 설정을 변경하여 임시 해결 방안을 찾을 수 있었으나, 이는 근본적인 해결책이 아니었습니다.

문제를 해결하기 위해 Ngrok을 사용하여 도메인, DNS, SSL 문제를 일괄 해결하였습니다. Ngrok은 또한 VM 방화벽 설정을 건드리지 않아도 되어 셀프호스팅 환경에서 유리했습니다.

### Insight
DuckDNS는 무료 DDNS 서비스라는 특성상 네임서버의 안정성이 낮아 중요한 환경에서의 사용은 적합하지 않습니다. 특히, 재현성이 중요한 환경에서는 문제가 될 수 있습니다. 

Ngrok은 이러한 점에서 보다 안정적이며, 셀프호스팅 입문자에게 적합한 대안이 될 수 있습니다. Ngrok을 통해 간단히 도메인/DNS/SSL 문제를 해결할 수 있었으며, 이는 편리함을 더해줍니다.

**출처:** [Let's Encrypt Community Forum](https://community.letsencrypt.org), [DuckDNS Official Documentation](https://www.duckdns.org)