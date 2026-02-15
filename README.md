# Random Key Generator 🔐

![Vue.js](https://img.shields.io/badge/Vue.js-35495E?style=for-the-badge&logo=vuedotjs&logoColor=4FC08D)
![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)

Um gerador de senhas e chaves aleatórias moderno, seguro e performático. Desenvolvido com **Vue 3** e **Tailwind CSS**, focado em privacidade e agilidade.

> **Acesse online:** [https://luizhanauer.github.io/rkey/](https://luizhanauer.github.io/rkey/)

---

## 📸 Preview

![<preview>](https://raw.githubusercontent.com/luizhanauer/rkey/assets/screenshot.png)

## ✨ Funcionalidades

* **Segurança em Primeiro Lugar:** Gerações realizadas 100% no navegador (Client-side) utilizando a API `crypto.getRandomValues()` para entropia criptograficamente segura. Nenhuma senha trafega pela internet.
* **Reatividade Instantânea:** A chave é atualizada em tempo real conforme você ajusta as configurações.
* **Customizável:** Controle total sobre o comprimento da chave, inclusão de números e símbolos.
* **Formatos Variados:** Copie a chave original, apenas maiúsculas ou apenas minúsculas com um clique.
* **Interface Limpa:** Design moderno e responsivo construído com Tailwind CSS.

## 🚀 Tecnologias

* [Vue.js 3](https://vuejs.org/) (Composition API & Script Setup)
* [TypeScript](https://www.typescriptlang.org/)
* [Tailwind CSS](https://tailwindcss.com/)
* [Vite](https://vitejs.dev/)

## 📦 Como rodar localmente

Diferente da versão anterior, este projeto agora utiliza um processo de build moderno. Para rodar o código fonte em sua máquina, você precisará do [Node.js](https://nodejs.org/) instalado.

1.  **Clone o repositório:**
    ```bash
    git clone https://github.com/luizhanauer/rkey.git

    cd rkey
    ```

2.  **Instale as dependências:**
    ```bash
    npm install
    ```

3.  **Rode o servidor de desenvolvimento:**
    ```bash
    npm run dev
    ```
    O projeto estará acessível em `http://localhost:5173`.

4.  **Para gerar a versão de produção:**
    ```bash
    npm run build
    ```

## 🤝 Contribuição

Contribuições são muito bem-vindas! Se você tiver sugestões de melhoria, novas features ou correções de bugs:

1.  Faça um Fork do projeto.
2.  Crie uma Branch para sua Feature (`git checkout -b feature/NovaFeature`).
3.  Faça o Commit (`git commit -m 'Add: Nova Feature'`).
4.  Faça o Push (`git push origin feature/NovaFeature`).
5.  Abra um Pull Request.

## ☕ Apoie o Projeto

Se este projeto foi útil para você ou economizou seu tempo, considere pagar um café! Isso ajuda a manter o desenvolvimento de ferramentas open source.

<a href="https://www.paypal.com/donate/?hosted_button_id=SFR785YEYHC4E" target="_blank">
  <img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 50px !important; width: 200px !important;" >
</a>

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.