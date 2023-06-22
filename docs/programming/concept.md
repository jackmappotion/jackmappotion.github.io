---
layout: default
title: concept
nav_order: 4
parent: Programming
---

# Concept
{: .no_toc }

{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## DeepLearning Related

> ### BackPropaGation(역전파)
> - 역전파의 본질을 Multi_layer model의 각 층들의 weight를 업데이트 하는데에 있다.
> - Single layer의 weight를 update할때 명확하던 방식을 체계화 한것임.


## Secure Related

> ### SSH(Secure Shell)

```
ssh-keygen
-> id_rsa, id_rsa.pub 
-> id_rsa는 private_key, id_rsa.pub public_key인데
실제로 local에 id_rsa.pub key가 필요한것은 아님

외부에 id_rsa.pub key를 주면 
로컬에서 ssh {ip_address} 이렇게 하면 id_rsa(즉 private_key를 통해 확인하고 그게 허용된 pub_key와 같은건지 보는거)
참고로
> ssh {ip_address} 
> 이게 ssh -i id_rsa {ip_address}임
> default로 id_rsa를 찾는거고 

뭐 만약 클라우드별로 키관리를 할꺼면

~.ssh > ls 
gcp_id gcp_id.pub
aws_id aws_id.pub
azure_id azure_id.pub

ssh -i gcp_id {gcp_id_address}
ssh -i aws_id {aws_id_address}
ssh -i azure_id {azure_id_address}

뭐 요런식으로 
```

## WEB

> ### WSGI
> - Web Server Gateway Interface

> ### ASGI
> - Asynchronous Server Gateway Interface
> - ASGI는 WSGI와 호환가능하다.
> - 비동기호출 가능
