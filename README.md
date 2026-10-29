# Autentication & Security

Um estudo meu sobre como fazer um login seguro e confiável. Montei um site simples de cadastro e login, com uma página de "segredos" que só aparece para quem está logado, e fui trocando a forma de guardar e conferir a senha, do jeito mais ingênuo até o uso de sessões.

As telas vieram do projeto inicial do curso [The Complete Web Development Bootcamp](https://www.udemy.com/course/the-complete-web-development-bootcamp/), da Angela Yu. A parte de segurança eu fui construindo em etapas, e cada etapa é um commit.

## As etapas

1. **Criptografia da senha.** A senha é criptografada no banco com `mongoose-encryption`, usando uma chave no `.env`. É melhor que texto puro, mas quem conseguir a chave consegue ler todas as senhas.
2. **Hash com MD5.** A senha deixa de ser criptografada e passa a ser um hash, que não tem volta: nem com a chave dá para recuperar a senha. Funciona, mas o MD5 é rápido demais: dá para testar bilhões de senhas por segundo, e senhas iguais geram o mesmo hash.
3. **bcrypt com salt.** Cada senha ganha um salt aleatório e passa por várias rodadas de hash. Senhas iguais viram hashes diferentes, e testar senha por senha fica lento de propósito.
4. **Sessões e cookies com Passport.** Depois do login, o usuário continua autenticado pela sessão (cookie), e a página de segredos confere isso antes de abrir. O `passport-local-mongoose` cuida do hash e do salt.

## O que eu aprendi

- A diferença entre criptografar e fazer hash: senha não deve poder ser recuperada, nem por quem tem a chave.
- Nunca guardar senha em texto puro, e não usar hash rápido (como MD5) para senha.
- Por que o salt importa e por que um hash lento é uma vantagem aqui.
- Como uma sessão mantém o usuário logado entre as páginas, e como proteger uma rota.
- Guardar segredos (como a chave da sessão) num `.env`, fora do código e do repositório.

## Tecnologias

Node.js, Express, EJS, MongoDB com Mongoose, bcrypt, Passport e express-session.

## Como rodar

Precisa do Node.js e de um MongoDB rodando em `localhost:27017`.

```bash
npm install
echo "SECRET=uma-frase-longa-e-aleatoria" > .env
node app.js
```

Depois é só abrir `http://localhost:3000`.
