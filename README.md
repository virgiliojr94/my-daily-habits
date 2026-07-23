# Projeto do Curso — DevOps & Cloud

App-base do **Módulo 11 (DevOps & Cloud)** da Capacitação Full Stack.
É a **aplicação-projeto de exemplo** do curso — um rastreador de hábitos bem simples, só pra ter
algo concreto pra levar do *"roda na minha máquina"* até *no ar, versionado, com CI e deploy automático*.
Cada professor pode trocar por seu próprio projeto: o que importa é o fluxo, não o app.

É um frontend **estático** (React + Vite), então não precisa de servidor nem banco:
o navegador guarda tudo. Isso é de propósito — deploy estático no GitHub Pages, custo zero.

---

## Rodar local (Aula 2 em diante)

```bash
npm install      # instala as dependências (uma vez)
npm run dev      # sobe em http://localhost:5173
npm test         # roda os testes (é o que o CI vai checar)
npm run build    # gera os estáticos em dist/
```

## Aula 4 — Conteinerizar com Docker

O `Dockerfile` é **multi-stage**: o estágio 1 compila a app com o Node; o estágio 2
copia só os estáticos pro nginx (imagem final pequena).

```bash
docker build -t projeto-curso .
docker run -p 8080:80 projeto-curso     # abre em http://localhost:8080
```

> O container é pra **aprender portabilidade** ("roda igual em qualquer máquina").
> O deploy público (Aula 5) NÃO usa o container — o Pages serve os estáticos direto.
> São dois caminhos diferentes de propósito.

## Aula 4/5 — CI: ver o pipeline ficar vermelho e depois verde

O workflow `.github/workflows/ci.yml` roda os testes a cada **Pull Request** pra `main`.

Experimento pra fazer em aula:
1. Numa branch, abra `src/habits.js` e quebre a `computeStreak` de propósito
   (ex: troque `streak += 1` por `streak += 2`).
2. Abra um Pull Request → o **Actions fica vermelho** (o teste de sequência falha).
3. Desfaça a mudança, faça commit → **fica verde**. Esse é o ciclo do CI.

## Aula 5 — Deploy Contínuo no GitHub Pages

Workflow `.github/workflows/deploy.yml`: quando algo entra na `main`, ele compila e
publica no Pages.

**Configuração única no repositório** (cada aluno faz uma vez):
`Settings → Pages → Build and deployment → Source: GitHub Actions`.

Depois de mergear na `main`, a app fica no ar em:
`https://SEU-USUARIO.github.io/SEU-REPO/` — o link pra mandar pra família.

## Aula 8 — Projeto Final

Em dupla, o fluxo completo de uma feature nova: `branch → Pull Request → CI (verde) →
merge → deploy automático`, e leitura dos logs do Actions como observabilidade.

---

## Mapa do repositório

| Arquivo | Pra que serve | Aula |
|---|---|---|
| `src/habits.js` | Lógica pura (testável) | 4 |
| `test/habits.test.js` | Testes que o CI roda | 4 |
| `Dockerfile` / `nginx.conf` | Conteinerização | 4 |
| `.github/workflows/ci.yml` | Integração Contínua | 4 |
| `.github/workflows/deploy.yml` | Entrega Contínua (Pages) | 5 |
| `src/App.jsx` / `App.css` | A interface da app | — |
