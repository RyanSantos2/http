# Relatório — Laboratório de Inspeção HTTP/HTTPS — Fluxo B (sem privilégio administrativo)

> **Como usar este template:** substitua os campos `[...]` pelas suas respostas,
> anexe as capturas de tela na pasta `evidencias/` e referencie-as onde indicado.
> Preserve a formatação markdown (tabelas, blocos de código) para facilitar a correção.
>
> **Observação:** toda a análise prática deste relatório é feita sobre tráfego **HTTP em texto claro**. A análise de HTTPS é teórica, baseada na fundamentação do `readme.md` do repositório.

---

## Identificação

| Campo       | Valor                  |
|-------------|------------------------|
| Nome        | Ryan Santos    |
| Nome        | João Marcelo    |
| Disciplina  | Redes de Computadores  |
| Turma       | SI II Ciclo         |
| Data        | 05/05/2026   |
| Fluxo       | **B — Aluno sem privilégio de administrador** |
| SO utilizado | [Windows 10 |
| Ferramenta de proxy | [Fiddler Classic per-user  |
| Navegador(es)       | [Chrome 148.0 |
| HTTPS-First Mode / HTTPS-Only desabilitado? | [sim / não] |

---

## Atividade 1 — Primeira captura (`http://example.com`)

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/90be406b-a5b4-43a2-8710-c1143f07c190" />



**Request-line enviada:**

```http
GET http://example.com/ HTTP/1.1
```

**Status-line recebida:**

```http
HTTP/1.1 304 Not Modified

```

### Pergunta 1.1
> Quantos cabeçalhos o navegador enviou no request? Liste-os.

**Resposta:**
9

Cabeçalhos:
Date: Wed, 06 May 2026 01:46:46 GMT
Connection: keep-alive
Allow: GET, HEAD
Age: 7717
Server: cloudflare
Last-Modified: Fri, 01 May 2026 01:24:29 GMT
etag: "69f400cd-210"
cf-cache-status: HIT
CF-RAY: 9f745025ea4c1b1d-GRU


### Pergunta 1.2
> Qual foi o `Content-Length` da resposta? Se ele não apareceu, registre `Transfer-Encoding`, versão do protocolo ou outro indício observado. O corpo retornado é HTML, texto puro, JSON ou binário? Como você descobriu?

Transfer-Encoding

---

## Atividade 2 — Anatomia de um GET (`http://httpbin.org/get?...`)

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/5f97279b-3c39-4765-a042-676e0688be3c" />


**Request-line completa:**

```http
GET http://httpbin.org/get?aluno=SEU_NOME&curso=redes HTTP/1.1
```

**Cabeçalhos-chave capturados:**

Host: httpbin.org
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/148.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: pt-BR,pt;q=0.9,en-US;q=0.8,en;q=0.7


**Campos do JSON de resposta:**

```json
{
  "args": {
    "aluno": "SEU_NOME", 
    "curso": "redes"
  }, 
  "headers": {
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7", 
    "Accept-Encoding": "gzip, deflate", 
    "Accept-Language": "pt-BR,pt;q=0.9,en-US;q=0.8,en;q=0.7", 
    "Host": "httpbin.org", 
    "Upgrade-Insecure-Requests": "1", 
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/148.0.0.0 Safari/537.36", 
    "X-Amzn-Trace-Id": "Root=1-69faa092-3e43bba25df8f561765b5694"
  }, 
  "origin": "152.243.125.150", 
  "url": "http://httpbin.org/get?aluno=SEU_NOME&curso=redes"

```

### Pergunta 2.1
> O valor do campo `origin` corresponde a qual elemento da rede? Por que normalmente não é o IP local?

Corresponde ao IP público. Pois é devido ao uso de NAT pelo roteador ou provedor de internet.

### Pergunta 2.2
> Compare o `User-Agent` enviado com o que aparece no JSON da resposta. Coincidem?

Sim, coincidem

### Pergunta 2.3
> Em `http://httpbin.org/headers`, liste até três cabeçalhos que o servidor vê mas **não aparecem** no Raw do request. De onde vêm? Se não encontrar três, explique por que o resultado pode variar.

Apenas um cabeçalho extra é visível em relação ao que o navegador enviou originalmente: X-Amzn-Trace-Id. O número de cabeçalhos extras depende inteiramente de quais e quantos intermediários existem no caminho entre computador e o servidor final

"X-Amzn-Trace-Id": "Root=1-69faa4a1-03a2430a500d650027738f00

---

## Atividade 3 — POST e envio de formulário (`http://httpbin.org/forms/post` → `/post`)

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/a5c91e63-b783-4807-b963-e377de8a6a51" />


**Request-line do POST:**

```http
POST http://httpbin.org/post HTTP/1.1
```

**Cabeçalhos do request:**

Content-Type: application/json
Content-Length: 1170

**Corpo completo do request:**

```
{
  "args": {}, 
  "data": "", 
  "files": {}, 
  "form": {
    "comments": "subir e bater na porta ", 
    "custemail": "teste@teste.com", 
    "custname": "ryan santos ", 
    "custtel": "13 34226531", 
    "delivery": "17:00", 
    "size": "small", 
    "topping": [
      "bacon", 
      "cheese", 
      "onion"
    ]
  }, 
  "headers": {
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7", 
    "Accept-Encoding": "gzip, deflate", 
    "Accept-Language": "pt-BR,pt;q=0.9,en-US;q=0.8,en;q=0.7", 
    "Cache-Control": "max-age=0", 
    "Content-Length": "173", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "httpbin.org", 
    "Origin": "http://httpbin.org", 
    "Referer": "http://httpbin.org/forms/post", 
    "Upgrade-Insecure-Requests": "1", 
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/148.0.0.0 Safari/537.36", 
    "X-Amzn-Trace-Id": "Root=1-69fb90f5-0e8135642c85bbac0d8f214c"
  }, 
  "json": null, 
  "origin": "152.243.125.150", 
  "url": "http://httpbin.org/post"
}

```

**Trecho do JSON de resposta (campo `form`):**

```json
"form": {
    "comments": "subir e bater na porta ", 
    "custemail": "teste@teste.com", 
    "custname": "ryan santos ", 
    "custtel": "13 34226531", 
    "delivery": "17:00", 
    "size": "small", 
    "topping": [
      "bacon", 
      "cheese", 
      "onion"
    ]
  }, 

```

### Pergunta 3.1
> Qual o formato do corpo? Como esse formato codifica caracteres especiais (espaço, acentos)?

Content-Type: application/x-www-form-urlencoded Funciona através de Percent-enconding (URL enconding)


### Pergunta 3.2
> Comparando **Request → WebForms** e **Request → Raw**: qual das duas corresponde literalmente aos bytes enviados no socket TCP?

Request -> **Request → Raw**


### Pergunta 3.3 — Composer
> Envie manualmente via Composer um `POST` para `http://httpbin.org/post` com JSON. Registre a resposta. Qual campo do JSON confirma que o servidor interpretou o JSON?

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/e632493e-d568-4609-ad8e-b7a4b1812288" />


**Response JSON (trecho relevante):**

```json
{
  {
  "args": {}, 
  "data": "", 
  "files": {}, 
  "form": {}, 
  "headers": {
    "Content-Length": "0", 
    "Content-Type": "application/json", 
    "Host": "httpbin.org", 
    "User-Agent": "Fiddler", 
    "X-Amzn-Trace-Id": "Root=1-69fb9933-5102fdd90c0dc69714138151"
  }, 
  "json": null, 
  "origin": "152.243.125.150", 
  "url": "http://httpbin.org/post"
}

}
```

**Resposta:** [...]

---

## Atividade 4 — Catálogo de status codes (`http://httpbin.org/...`)

**Captura de tela (lista do Fiddler com as 7 sessões):** `evidencias/atv4_lista.png`

| # | Método | URL | Status-line | `Content-Length` / `Transfer-Encoding` | Body presente? |
|---|--------|-----|-------------|-----------------------------------------|----------------|
| 1 | GET    | `http://httpbin.org/status/200` | [...] | [...] | [sim/não] |
| 2 | GET    | `http://httpbin.org/redirect-to?status_code=301&url=/get` | [...] | [...] | [sim/não] |
| 3 | GET    | `http://httpbin.org/status/404` | [...] | [...] | [sim/não] |
| 4 | GET    | `http://httpbin.org/status/418` | [...] | [...] | [sim/não] |
| 5 | GET    | `http://httpbin.org/status/500` | [...] | [...] | [sim/não] |
| 6 | GET    | `http://httpbin.org/status/503` | [...] | [...] | [sim/não] |
| 7 | GET    | `http://example.com/` com `If-Modified-Since` | [...] | [...] | [sim/não] |

### Pergunta 4.1
> Em qual dos status o corpo está ausente/tamanho zero? Isso é obrigatório pela especificação ou depende do servidor?

**Resposta:** [...]

### Pergunta 4.2
> No `301`, qual cabeçalho da resposta informa para onde ir? O que aconteceria se estivesse ausente?

**Resposta:** [...]

### Pergunta 4.3
> Diferença semântica entre `200`, `304` e `404` do ponto de vista do cache do navegador.

**Resposta:** [...]

---

## Atividade 5 — Identificação de cabeçalhos (`http://httpbin.org/response-headers?...` + `/gzip`)

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/502408f9-7a72-46ef-a37a-e0d13e801e21" />
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/7032b33d-11f4-4b83-8c93-03b515a8462d" />



| Cabeçalho                    | Req/Resp | Valor capturado | Função em uma frase |
|------------------------------|----------|------------------|----------------------|
| `Host`                       | Req    | httpbin.org        | Especifica o domínio do servidor alvo da requisição. |
| `User-Agent`                 | Req    | Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/148.0.0.0 Safari/537.36 | Identifica o software cliente e o sistema operacional fazendo a requisição. |
| `Accept`                     | Req    | text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7| Informa ao servidor os tipos de mídia (formatos) que o cliente consegue entender. |
| `Accept-Encoding`            | Req    | gzip, deflate      | Indica quais algoritmos de compressão o cliente suporta para a resposta. |
| `Cookie`                     | Req    | teste=1            | Envia de volta ao servidor dados de estado previamente armazenados pelo cliente. |
| `Server`                     | Resp   | gunicorn/19.9.0    | Identifica o software do servidor web que gerou a resposta. |
| `Content-Type`               | Resp   | application/json   | Indica o tipo de mídia (formato) do corpo da mensagem que está sendo enviada. |
| `Content-Encoding`           | Resp   | gzip               | Indica qual algoritmo de compressão foi aplicado ao corpo da resposta. |
| `Set-Cookie`                 | Resp   | teste=1            | Instrução do servidor para o cliente armazenar um cookie em sua máquina. |
| `Cache-Control`              | Resp   | max-age=3600       | Define diretivas e regras sobre como a resposta deve ser armazenada em cache. |
| `Strict-Transport-Security`  | Resp   | Ausente            | Força o cliente a se comunicar com o servidor apenas via conexão segura (HTTPS). |

### Pergunta 5.1
> `Content-Encoding: gzip`/`br` apareceu? Compare `Content-Length`, quando presente, com o conteúdo visível. O que explica a diferença?

**Resposta:** Sim, na requisição para o endpoint `/gzip`, o servidor retornou `Content-Encoding: gzip`. O valor do `Content-Length` (tamanho em bytes transmitido pela rede) é menor do que o tamanho do texto visível na aba **TextView**. Isso ocorre porque o `Content-Length` representa o tamanho do arquivo **compactado** que trafegou na rede.

### Pergunta 5.2
> Cliente envia `Accept: application/json` mas o recurso só existe em `text/html`. Qual status code esperar?

**Resposta:** Segundo a especificação HTTP, se o servidor não for capaz de fornecer uma resposta com as características de formato aceitas pelo cliente (declaradas no cabeçalho `Accept`), ele deve retornar o código de status **`406 Not Acceptable`**. Isso indica ao cliente que o recurso existe, mas não no formato que o cliente declarou conseguir processar.

### Pergunta 5.3
> `Strict-Transport-Security` apareceu nas respostas HTTP? Por que esse cabeçalho está ausente neste fluxo? (Consulte a RFC 6797.) Qual é seu papel contra downgrades para HTTP puro?

**Resposta:** Não, o cabeçalho não apareceu. Ele está ausente porque todas as requisições destes testes foram feitas via protocolo **HTTP puro (não criptografado)** no endereço `http://httpbin.org`. Segundo a RFC 6797, os servidores não devem enviar o cabeçalho HSTS em conexões HTTP inseguras (e se enviarem, o cliente deve ignorar, pois a rede não é confiável). 
O papel do HSTS (HTTP Strict Transport Security) é proteger os usuários contra ataques de *downgrade* (como o SSL Stripping). Uma vez que o cliente recebe esse cabeçalho através de uma conexão HTTPS válida, ele memoriza que aquele domínio específico só pode ser acessado via HTTPS. A partir daí, qualquer tentativa futura de acessar o site via `http://` será bloqueada ou convertida automaticamente para `https://` pelo próprio navegador de forma interna, sem nem deixar a requisição insegura sair para a rede.

---

## Atividade 6 — HTTP vs HTTPS (análise sem decriptação)

**Captura de tela HTTP (`neverssl.com`):**
<img width="1910" height="1074" alt="neverssl.com" src="https://github.com/user-attachments/assets/1f17ab5d-0f7a-47a8-955f-267c6a7a60f1" /><br>

**Captura de tela HTTPS (`https://httpbin.org/get`, apenas CONNECT):**
<img width="1919" height="1079" alt="httpbin.org/get" src="https://github.com/user-attachments/assets/e6713213-d689-46a1-9e6e-c886815ff603" /><br>

### Pergunta 6.1
> Que método HTTP aparece na sessão do `https://httpbin.org/get`? O que ele faz e por que existe?

**Resposta:** O método que aparece é o CONNECT. Ele existe para permitir que o cliente solicite ao proxy a criação de um túnel TCP bidirecional direto com o servidor de destino (na porta 443). Uma vez que esse túnel é estabelecido (com a resposta 200 Connection Established), o proxy para de ler os cabeçalhos HTTP e passa a apenas repassar os bytes criptografados de um lado para o outro, sem saber o conteúdo da comunicação.

### Pergunta 6.2
> Tabela comparativa dos campos visíveis ao Fiddler em cada caso:

| Campo                          | Visível em HTTP? | Visível em HTTPS (sem decriptação)? |
|--------------------------------|------------------|-------------------------------------|
| Método                         | Sim              | Não (apenas o método CONNECT inicial) |
| URL completa (path + query)    | Sim              | Não                                   |
| Cabeçalhos de request          | Sim              | Não                                   |
| Corpo de request               | Sim              | Não                                   |
| Status code                    | Sim              | Não (apenas o 200 Connection Established do túnel) |
| Cabeçalhos de response         | Sim              | Não                                   |
| Corpo de response              | Sim              | Não (apenas bytes cifrados/ilegíveis) |
| Host (via SNI, no `CONNECT`)   | Sim              | Sim                                   |
| IP e porta de destino          | Sim              | Sim                                   |

### Pergunta 6.3 (teórica)
> O que você **veria** no Fiddler se tivesse privilégio de administrador e pudesse habilitar *Decrypt HTTPS traffic*? Indique telas/abas e justifique por que essa inspeção exige a instalação de um certificado raiz.

**Resposta:** Veríamos o tráfego HTTPS em texto claro, exatamente como ocorre no HTTP puro. A URL completa e os cabeçalhos apareceriam legíveis na aba Inspectors → Raw, e o corpo das requisições/respostas ficaria visível nas abas Response → JSON / TextView. Essa inspeção exige a instalação de um certificado raiz porque o Fiddler atua como um Man-in-the-Middle (MITM), interceptando a conexão e emitindo certificados "falsos" para cada site visitado. Para que o navegador aceite esses certificados falsos sem bloquear a navegação, o certificado raiz do Fiddler precisa ser explicitamente adicionado ao armazenamento de confiança do sistema operacional.

### Pergunta 6.4
> Por que a técnica de decriptação dos *debugging proxies* **não** funcionaria contra um usuário se um atacante a tentasse sem instalar o certificado?

**Resposta:** Porque a criptografia HTTPS baseia-se em uma cadeia de confiança. Sem a "cooperação do usuário" (que é a instalação voluntária do certificado raiz do proxy/atacante na máquina), o ataque falha. Se um atacante tentar interceptar a conexão e enviar seus certificados "falsos" sem essa raiz de confiança instalada, o navegador da vítima detectará imediatamente que o certificado não foi emitido por uma Autoridade Certificadora confiável e bloqueará o acesso à página exibindo um alerta severo de segurança, impedindo o vazamento de dados.

---

## Atividade 7 — Cookies e sessão (`http://httpbin.org/cookies/...`)

**Captura de tela da sequência:** `evidencias/atv7_cookies.png`

| # | URL | `Set-Cookie` recebido | `Cookie` enviado |
|---|-----|-----------------------|-------------------|
| 1 | `/cookies/set?...`       | [...] | [nenhum / ...] |
| 2 | `/cookies` (1ª visita)   | [...] | [...]          |
| 3 | `/cookies` (reload 1)    | [...] | [...]          |
| 4 | `/cookies` (reload 2)    | [...] | [...]          |

### Pergunta 7.1
> `Set-Cookie` aparece uma vez ou em toda requisição? Justifique.

**Resposta:** [...]

### Pergunta 7.2
> Que atributos o `Set-Cookie` trouxe? Explique cada um presente. Para atributos não observados, registre `não observado`.

**Resposta:**

| Atributo | Valor | Função |
|----------|-------|--------|
| [...]    | [...] | [...]  |

### Pergunta 7.3
> O atributo `Secure` pode aparecer num cookie recebido por HTTP puro? Qual seria o comportamento esperado? Relacione com o fato de que todo o tráfego desta atividade é visível em texto claro.

**Resposta:** [...]

### Pergunta 7.4
> Na aba **Inspectors → Cookies**, o cookie armazenado coincide com o campo `cookies` do JSON?

**Resposta:** [...]

---

## Atividade 8 — Manipulação com breakpoints

**Captura de tela da edição do User-Agent:**
<img width="1169" height="587" alt="image" src="https://github.com/user-attachments/assets/52282bb2-64ff-44b4-8fe8-531e3f992bcc" /><br>
<img width="940" height="570" alt="image" src="https://github.com/user-attachments/assets/cd61eefe-59e1-4bf0-b096-5709b528f1da" /><br>

**JSON de resposta após edição:**

```json
{
  "user-agent": "LaboratorioRedes/1.0 (Guilherme)"
}
```

### Pergunta 8.1
> O servidor pode detectar que o `User-Agent` foi forjado? Discuta.

**Resposta:** Não, o servidor não tem como detectar de forma determinística que o cabeçalho `User-Agent` foi forjado apenas analisando esse campo. O protocolo HTTP é baseado em texto e confia cegamente nas informações declaradas pelo cliente. O servidor processa os cabeçalhos exatamente como os recebe. Embora um servidor mais avançado possa usar heurísticas (como analisar a assinatura de pacotes TCP/IP) para suspeitar que a requisição não veio de um navegador comum, ele não pode provar que a string do `User-Agent` foi alterada no meio do caminho sem o uso de mecanismos adicionais de integridade e criptografia que validem a mensagem do cliente de ponta a ponta.

### Pergunta 8.2
> Após editar a status-line de `200 OK` para `404 Not Found`, o que o navegador exibe? Comente o papel do proxy como MITM.

**Captura de tela:**
<img width="961" height="1033" alt="image" src="https://github.com/user-attachments/assets/4b36f984-e331-4474-8d77-1f2de7dc018b" /><br>

**Resposta:** O navegador exibe uma página de erro (ou mensagem do próprio navegador) indicando que a página não foi encontrada (404 Not Found). 
A manipulação afetou apenas a visualização local porque o servidor remoto (`httpbin.org`) processou a requisição e retornou, de fato, a resposta de sucesso original (`200 OK`). O servidor não tem ciência da falsificação. Isso demonstra o poder do proxy como um atacante *Man-in-the-Middle* (MitM). Ao interceptar a comunicação antes de ela chegar ao destino final (o navegador), o proxy consegue alterar completamente a "realidade" percebida pelo cliente. O cliente recebe e confia na resposta adulterada pelo proxy, provando que um MitM não apenas espiona passivamente, mas tem controle ativo bidirecional sobre o fluxo de dados em texto claro.

### Pergunta 8.3
> Confirme que todos os breakpoints foram desabilitados.

- [ X ] Breakpoints desabilitados ao final (Shift+F11)

---

## Atividade 9 — Redirecionamento HTTP → HTTPS

**Captura de tela:** 
<img width="960" height="1022" alt="image" src="https://github.com/user-attachments/assets/2b074b19-5a34-4d5b-bf41-3c92455604be" />


**Status-line da resposta a `http://httpbin.org/redirect-to?status_code=301&url=https%3A%2F%2Fhttpbin.org%2Fget`:**

```http
HTTP/1.1 301 MOVED PERMANENTLY
```

**Cabeçalho `Location` da resposta:**

```
Location: [https://httpbin.org/get](https://httpbin.org/get)
```

### Pergunta 9.1
> Código de status e cabeçalho que direcionaram o navegador para `https://`.

**Resposta:** O código de status retornado pelo servidor foi o 301 MOVED PERMANENTLY. O cabeçalho responsável por direcionar o navegador para a versão segura da página foi o Location (especificamente entregando a URL de destino [https://httpbin.org/get](https://httpbin.org/get))

### Pergunta 9.2
> Além do redirecionamento 3xx, qual outro mecanismo/cabeçalho faz o navegador passar a forçar HTTPS em visitas futuras? Cite a RFC.

**Resposta:** O outro mecanismo é o HSTS (HTTP Strict Transport Security). Esse mecanismo é ativado através do cabeçalho de resposta Strict-Transport-Security, que é formalmente definido pela RFC 6797. Uma vez que o navegador recebe e armazena esse cabeçalho (através de uma conexão HTTPS válida), ele memoriza que o domínio exige segurança. A partir desse momento, o próprio navegador força a conversão de qualquer URL http:// para https:// internamente, antes mesmo de enviar a requisição para a rede.

### Pergunta 9.3
> Se esse cabeçalho fosse enviado por uma resposta servida via HTTP puro, o navegador deveria obedecer? Justifique com base na RFC.

**Resposta:** Não, o navegador não deve obedecer. De acordo com as diretrizes de segurança da RFC 6797, os agentes de usuário (navegadores) são explicitamente instruídos a ignorar o cabeçalho Strict-Transport-Security se ele for recebido por meio de uma conexão HTTP não criptografada. A justificativa para isso é que, em um canal inseguro (texto claro), qualquer atacante posicionado como Man-in-the-Middle poderia facilmente forjar, injetar ou remover esse cabeçalho de forma maliciosa.

---

## Questões de Verificação

### 1. Ordem dos elementos em uma mensagem HTTP/1.1. O que separa cabeçalhos do corpo?

[resposta]

### 2. Por que `Host` é obrigatório em HTTP/1.1 mas era opcional em HTTP/1.0?

[resposta]

### 3. Diferença entre `401 Unauthorized` e `403 Forbidden`.

[resposta]

### 4. Um `POST` enviado duas vezes produz o mesmo efeito? E um `PUT`? Justifique em termos de idempotência.

[resposta]

### 5. Por que HTTPS permite ainda que um observador saiba qual site está sendo visitado? (SNI, DNS)

[resposta]

### 6. O que muda com `Content-Encoding: gzip`? Onde os dados são compactados e descompactados?

[resposta]

### 7. Impacto prático de `Cache-Control: no-store`.

[resposta]

### 8. Como um debugging proxy decifra HTTPS sem violar a criptografia, e por que isso exige cooperação do usuário (e por que, justamente, você não pôde executar essa etapa)?

[resposta]

### 9. Exemplo de cabeçalho de request que o navegador envia automaticamente, sem a página pedir.

[resposta]

### 10. Se fosse automatizar a inspeção via script, qual ferramenta alternativa escolheria? Por quê?

[resposta]

### 11. (Exclusiva do Fluxo B) Três cabeçalhos de segurança que não aparecem ou não fazem sentido em respostas HTTP puro. Para cada um, o que aconteceria se enviado por um servidor HTTP? (Cite RFC 6797 para HSTS.)

**Resposta:**

| Cabeçalho | Comportamento esperado sobre HTTP | Referência |
|-----------|-----------------------------------|-----------|
| [...]     | [...]                             | [...]     |
| [...]     | [...]                             | [...]     |
| [...]     | [...]                             | [...]     |

---

## Reflexão final (opcional, até 10 linhas)

> O que você aprendeu que não conhecia antes deste laboratório? Há algum
> cabeçalho, código de status ou comportamento que passou a olhar com
> mais atenção? Alguma dificuldade que recomendaria evitar para a próxima turma?

[reflexão]

---

## Encerramento — justificativa de segurança (Fluxo B)

**Parágrafo: por que a remoção de certificado é dispensável neste fluxo e por que seria obrigatória para o aluno administrador:**

[redigir, em até 5 linhas, com base na seção 4.6 do readme.md]

- [ ] HTTPS-First Mode / HTTPS-Only Mode reabilitado no navegador
- [ ] Fiddler / mitmproxy / HTTP Toolkit fechado (porta de proxy liberada)
- [ ] Configuração de proxy removida do navegador (se aplicável)

---

## Checklist de entrega

- [ ] Todos os campos `[...]` substituídos
- [ ] Pasta `evidencias/` com capturas nomeadas por atividade (incluindo Atv. 9)
- [ ] 11 questões de verificação respondidas
- [ ] Atividade 9 (redirecionamento HTTP→HTTPS) documentada
- [ ] Justificativa de encerramento redigida
- [ ] Arquivo compactado como `NOME_RA_LAB_HTTP_FLUXOB.zip`
- [ ] Submetido no Microsoft Teams dentro do prazo
