# Política de Segurança

Segurança é o produto. O Aedis é uma plataforma **segura por construção**, e tratamos a segurança do próprio projeto com o mesmo rigor — controles ligados por padrão e uma cadeia de *supply-chain* verificável.

> Esta política vale como padrão para **todos os repositórios** da organização [`aedis-build`](https://github.com/aedis-build). Um repositório pode publicar a sua própria `SECURITY.md` para sobrepor este default.

## Reportando uma vulnerabilidade

**Não abra uma issue pública** para vulnerabilidades de segurança.

Use um dos canais privados:

1. **GitHub Private Vulnerability Reporting** — na aba **Security** do repositório afetado → **"Report a vulnerability"**. É o canal preferencial.
2. **E-mail** — `security@aedis.build` (até o domínio estar ativo, use o reporting privado do GitHub acima).

Inclua, sempre que possível:

- componente/pacote e versão afetados;
- descrição do impacto e do cenário de exploração;
- passos de reprodução (PoC minimal) e configuração;
- mitigação ou correção sugerida, se houver.

**Compromisso de resposta:**

| Etapa | Prazo alvo |
|---|---|
| Confirmação de recebimento | até 3 dias úteis |
| Avaliação inicial e severidade | até 7 dias úteis |
| Correção / mitigação coordenada | conforme severidade (críticas priorizadas) |

Pedimos **divulgação coordenada**: dê-nos tempo razoável para corrigir antes de tornar a falha pública. Reconhecemos publicamente quem reporta de forma responsável (salvo pedido de anonimato).

## Versões suportadas

O Aedis está em caminho para a **v1.0 (GA)**. Antes do GA, apenas a linha mais recente recebe correções de segurança.

| Versão | Suporte |
|---|---|
| `main` / pré-release mais recente | ✅ |
| Pré-releases anteriores | ❌ |

Após o GA, a política de versionamento (semver) e a janela de suporte serão publicadas no repositório do framework.

## Postura segura por construção

Para ser inseguro, seria preciso trabalhar **contra** a plataforma. Controles ligados por padrão:

- **Edge** — headers OWASP, `Server` oculto, validação de host, *forwarded headers* sem *trust* implícito.
- **AuthN/AuthZ** — JWT/OIDC com validação de *issuer/audience/lifetime/signing key*, *clock skew* controlado, *policies* por role.
- **Input** — validação automática → respostas `422` estruturadas.
- **Erros** — taxonomia de violações → **RFC 9457 ProblemDetails**, sem vazar internals.
- **Concorrência** — idempotência e *distributed lock* contra duplicidade e *race conditions*.
- **Resiliência** — *retry* com *backoff*, DLQ e compensação (Saga) para falhas distribuídas.

## Cadeia de supply-chain verificável

Cada etapa produz um **artefato auditável** e atua como *gate* de release:

- **SAST** (ex.: CodeQL) e **SCA** de dependências (alertas de CVE) bloqueiam merge em achados *high/critical*.
- **Secret scanning** bloqueia segredos em commits.
- **SBOM** (CycloneDX/SPDX) publicado por pacote.
- **Build determinístico** + **SourceLink** para reprodutibilidade.
- **Assinatura de pacote** e **build provenance** (SLSA) como pré-requisito de publicação.
- **OpenSSF Scorecard** monitorando a postura do repositório.

## Boas práticas para quem consome o Aedis

- Mantenha os pacotes `Aedis.*` atualizados; aplique *patches* de segurança rapidamente (o design *semver* + *shim* de transição reduz o *blast radius*).
- Não desabilite os controles padrão sem entender o trade-off.
- Gerencie segredos por um *secrets manager* (Vault/cloud), nunca em código ou em texto plano.

---

Obrigado por ajudar a manter o ecossistema Aedis seguro. 🦫🛡️
