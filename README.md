# 📋 Sobre a atividade

Vamos criar a troca de tema dark / light para entender o uso de Context API com styled-component.

# 🖖🏻 Mão na massa!

Documentação: [Styled-Components](https://styled-components.com/docs/advanced)

Provider:

A biblioteca styled-components tem o componente auxiliar <ThemeProvider> que usa a props theme para compartilhar as configurações de estilo com os outros componentes.

### Entendendo os arquivos:

*   Componentes:
    *   AppContainer
    *   MainSection
    *   Button

### Biblioteca que vamos usar:

*   Styled Components (adicione no projeto com o comando: `yarn add styled-components`)

### Estrutura dos arquivos:

    .
    ├── README.md
    ├── node_modules/
    │   └──(...)
    ├── package.json
    ├── public/
    │   └── index.html
    ├── src/
    │   ├── components/
    │   │   └── app.style.js
    │   │   └── button.style.js
    │   ├── App.js
    │   ├── index.js
    │   ├── themes.js
    └── yarn.lock

`app.style.js`

    // Importando o styled-components para a variável styled 
    import styled from "styled-components";

    //Estilização do componente AppContainer
    export const AppContainer = styled.div`
      height: 37vh;
      display: flex;
      justify-content: center;
      padding: 300px;

    // Abaixo passamos para as propriedades background-color e color as configurações de estilo definidas no arquivo theme.js e compartilhada pela props theme do componente <ThemeProvider>

      background-color: ${(props) => props.theme.backgroundColor};
      color: ${(props) => props.theme.color};
      transition: background-color 0.8s linear, color 0.5s linear;
    `;

    //Estilização do componente MainSection
    export const MainSection = styled.div`
    text-align: center;
    `;

`button.style.js`

    // Importando o styled-components para a variável styled 
    import styled from "styled-components";

    //Estilização do componente Button. Nas propriedades que precisam ser dinamicas usamos a prop theme disponibilizada pelo componente <ThemeProvider>
    export const Button = styled.button`
      outline: none;
      border: 1px solid;
      border-color: ${(props) => props.theme.button.borderColor};
      border-radius: 2px;
      background-color: ${(props) => props.theme.button.backgroundColor};
      color: ${(props) => props.theme.button.textColor};
      padding: 0.5em 1em;
      box-shadow: 5px 2px 20px 5px rgba(21, 26, 105, 1);

      font-size: 1rem;
      letter-spacing: 0.7px;
      cursor: pointer;
      transition: opacity 0.4s linear, color 0.4s linear,
        background-color 0.4s linear;

      &:hover {
        opacity: 0.65;
      }
    `;

`themes.js`

    // Objeto com a definição do estilo para o tema lightTheme
    const lightTheme = {
      color: "#0e14e",
      backgroundColor: "#D3D4E6",
      button: {
        textColor: "#D3D4E6",
        borderColor: "#151A69",
        backgroundColor: "#151A69",
      },
    };

    // Objeto com a definição do estilo para o tema darkTheme
    const darkTheme = {
      color: "#D3D4E6",
      backgroundColor: "#39424E",
      button: {
        textColor: "#151A69",
        borderColor: "#151A69",
        backgroundColor: "#39424E",
      },
    };

    // Exportando as definições de estilo (lightTheme e darkTheme) no objeto themes, que será usado na props theme do componente auxiliar <ThemeProvider>
    export const themes = {
      light: lightTheme,
      dark: darkTheme,
    };

`App.js`

    import { useCallback, useState } from "react";
    import { AppContainer, MainSection } from "./components/app.style";
    import { ThemeProvider } from "styled-components";
    import { themes } from "./themes";
    import { Button } from "./components/button.style";

    function App() {
    // useState que armazena o tema atual da aplicação
      const [currentTheme, setCurrentTheme] = useState("light");

    // variável que armazena o text do tema que não está sendo usado no momento, para personalizar o texto do botão
      const getOpositeTheme = useCallback(
        () => (currentTheme === "light" ? "dark" : "light"),
        [currentTheme]
      );

      return (
    // <ThemeProvider> é o componente auxiliar da biblioteca styled-components que prove as informações do tema atual atraves da pros theme para todos componentes que estão dentro dele.
        <ThemeProvider theme={themes[currentTheme]}>
          <AppContainer>
            <MainSection>
              <h1>You are in {currentTheme} mode</h1>
              <Button onClick={() => setCurrentTheme(getOpositeTheme())}>
                switch to {getOpositeTheme()} mode
              </Button>
            </MainSection>
          </AppContainer>
        </ThemeProvider>
      );
    }

    export default App;
