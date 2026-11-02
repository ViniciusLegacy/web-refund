# 💸 Refund

Aplicação web para **solicitação e gerenciamento de reembolsos de despesas** de uma empresa. Funcionários enviam pedidos de reembolso com categoria, valor e comprovante; gestores visualizam e analisam essas solicitações em um painel.

> 📚 Projeto desenvolvido com fins de **estudo e prática** de React, TypeScript, Tailwind CSS, React Router e boas práticas de componentização no front-end.

---

## 📑 Sumário

- [Funcionalidades](#-funcionalidades)
- [Tecnologias](#-tecnologias)
- [Estrutura do projeto](#-estrutura-do-projeto)
- [Como rodar o projeto](#-como-rodar-o-projeto)
- [Scripts disponíveis](#-scripts-disponíveis)
- [Rotas e perfis de acesso](#-rotas-e-perfis-de-acesso)
- [Conceitos praticados](#-conceitos-praticados)
- [Status do projeto](#-status-do-projeto)
- [Autor](#-autor)

---

## ✨ Funcionalidades

- **Autenticação (telas):** login e cadastro de usuários.
- **Solicitação de reembolso (funcionário):**
  - Formulário com nome da solicitação, categoria, valor e upload de comprovante.
  - Tela de confirmação após o envio (acessível apenas a partir do formulário).
- **Painel de solicitações (gestor):**
  - Listagem das solicitações com ícone da categoria e valor formatado em Real (BRL).
  - Busca por nome.
  - Paginação.
  - Visualização dos detalhes de uma solicitação em modo somente leitura, com link para o comprovante.
- **Categorias de despesa:** Alimentação, Hospedagem, Transporte, Serviços e Outros.
- **Roteamento por perfil:** as rotas disponíveis mudam conforme o papel do usuário (`employee`, `manager` ou não autenticado).
- **Componentes de feedback:** tela de carregamento (`Loading`) e página 404 (`NotFound`).
- **Layout responsivo** construído com Tailwind CSS.

---

## 🚀 Tecnologias

| Tecnologia | Uso no projeto |
| --- | --- |
| [React 19](https://react.dev/) | Biblioteca para construção da interface |
| [TypeScript](https://www.typescriptlang.org/) | Tipagem estática |
| [Vite](https://vite.dev/) | Ferramenta de build e servidor de desenvolvimento |
| [Tailwind CSS 4](https://tailwindcss.com/) | Estilização com classes utilitárias e tema customizado (`@theme`) |
| [React Router](https://reactrouter.com/) | Roteamento da aplicação |
| [clsx](https://github.com/lukeed/clsx) | Composição condicional de classes CSS |
| [tailwind-merge](https://github.com/dcastil/tailwind-merge) | Resolução de conflitos entre classes do Tailwind |

---

## 📁 Estrutura do projeto

```
web-refund/
├── public/                 # Arquivos estáticos servidos diretamente
├── src/
│   ├── assets/             # Ícones e imagens (SVG)
│   ├── components/         # Componentes reutilizáveis
│   │   ├── AppLayout.tsx   # Layout das páginas autenticadas
│   │   ├── AuthLayout.tsx  # Layout das páginas de login/cadastro
│   │   ├── Button.tsx      # Botão com variantes e estado de carregamento
│   │   ├── Header.tsx      # Cabeçalho da aplicação
│   │   ├── Input.tsx       # Campo de texto com legenda
│   │   ├── Loading.tsx     # Indicador de carregamento
│   │   ├── Pagination.tsx  # Controles de paginação
│   │   ├── RefundItem.tsx  # Item da listagem de reembolsos
│   │   ├── Select.tsx      # Campo de seleção com legenda
│   │   └── Upload.tsx      # Campo de upload de comprovante
│   ├── pages/              # Páginas da aplicação
│   │   ├── Confirm.tsx     # Confirmação de envio da solicitação
│   │   ├── Dashboard.tsx   # Painel de solicitações (gestor)
│   │   ├── NotFound.tsx    # Página 404
│   │   ├── Refund.tsx      # Formulário / detalhes da solicitação
│   │   ├── SignIn.tsx      # Login
│   │   └── SignUp.tsx      # Cadastro
│   ├── routes/             # Definição das rotas
│   │   ├── index.tsx           # Seleciona as rotas de acordo com o perfil
│   │   ├── auth-routes.tsx     # Rotas públicas (login/cadastro)
│   │   ├── employee-routes.tsx # Rotas do funcionário
│   │   └── manager-routes.tsx  # Rotas do gestor
│   ├── utils/              # Funções e constantes auxiliares
│   │   ├── categories.ts       # Categorias de despesa e seus ícones
│   │   ├── classMerge.ts       # Helper que combina clsx + tailwind-merge
│   │   └── formatCurrency.ts   # Formatação de valores em BRL
│   ├── App.tsx             # Componente raiz
│   ├── index.css           # Importação do Tailwind e tema customizado
│   └── main.tsx            # Ponto de entrada da aplicação
├── index.html
├── package.json
├── tsconfig.json
└── vite.config.ts
```

---

## 🛠️ Como rodar o projeto

### Pré-requisitos

- [Node.js](https://nodejs.org/) **20.19+** ou **22.12+** (exigido pelo Vite)
- npm (instalado junto com o Node.js)
- [Git](https://git-scm.com/)

### Passo a passo

```bash
# 1. Clone o repositório
git clone <url-do-repositorio>

# 2. Acesse a pasta do projeto
cd web-refund

# 3. Instale as dependências
npm install

# 4. Inicie o servidor de desenvolvimento
npm run dev
```

A aplicação ficará disponível em **http://localhost:5173**.

---

## 📜 Scripts disponíveis

| Comando | Descrição |
| --- | --- |
| `npm run dev` | Inicia o servidor de desenvolvimento com hot reload |
| `npm run build` | Verifica os tipos com o TypeScript e gera o build de produção em `dist/` |
| `npm run preview` | Serve localmente o build de produção |

---

## 🔐 Rotas e perfis de acesso

As rotas são escolhidas em [`src/routes/index.tsx`](src/routes/index.tsx) com base no papel (`role`) do usuário na sessão:

| Perfil | Rota | Página |
| --- | --- | --- |
| Não autenticado | `/` | Login |
| Não autenticado | `/signup` | Cadastro |
| Funcionário (`employee`) | `/` | Nova solicitação de reembolso |
| Funcionário (`employee`) | `/confirm` | Confirmação de envio |
| Gestor (`manager`) | `/` | Painel de solicitações |
| Gestor (`manager`) | `/refund/:id` | Detalhes de uma solicitação |
| Todos | `*` | Página não encontrada (404) |

> 💡 Por enquanto a sessão é simulada no código. Para visualizar as telas de cada perfil, altere o valor de `session.user.role` em [`src/routes/index.tsx`](src/routes/index.tsx) para `"employee"` ou `"manager"`.

---

## 🧠 Conceitos praticados

- Componentização e reaproveitamento de componentes
- Tipagem de props com TypeScript, estendendo props nativas (`React.ComponentProps<"button">`)
- Componentes com variantes de estilo
- Gerenciamento de estado com `useState`
- Formulários controlados e tratamento de eventos
- Roteamento com rotas aninhadas, layouts (`<Outlet />`), parâmetros (`useParams`) e navegação programática (`useNavigate`)
- Passagem de estado entre rotas (`location.state`) e redirecionamento com `<Navigate />`
- Separação de rotas por perfil de usuário
- Tema customizado no Tailwind CSS 4 com `@theme` (cores, fonte e tamanhos)
- Mesclagem de classes com `clsx` + `tailwind-merge`
- Formatação de moeda com `Intl.NumberFormat`
- Layout responsivo

---

## 🚧 Status do projeto

Em desenvolvimento. Neste momento o projeto é **apenas front-end**: os dados exibidos são de exemplo e a autenticação é simulada.

Próximos passos possíveis:

- [ ] Integração com uma API back-end
- [ ] Autenticação real e contexto de sessão
- [ ] Envio e armazenamento real dos comprovantes
- [ ] Validação de formulários
- [ ] Testes automatizados

---

## 👨‍💻 Autor

Desenvolvido por **ViniciusLegacy** para fins de estudo.
