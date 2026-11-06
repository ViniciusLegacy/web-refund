# 💸 Refund

Aplicação web para **solicitação e gerenciamento de reembolsos de despesas** de uma empresa. Funcionários enviam pedidos de reembolso com categoria, valor e comprovante; gestores visualizam e analisam essas solicitações em um painel.

O front-end consome uma **API REST** (back-end separado) para autenticação, cadastro de usuários, envio de comprovantes e gerenciamento das solicitações.

> 📚 Projeto desenvolvido com fins de **estudo e prática** de React, TypeScript, Tailwind CSS, React Router, consumo de APIs e boas práticas de componentização no front-end.

---

## 📑 Sumário

- [Funcionalidades](#-funcionalidades)
- [Tecnologias](#-tecnologias)
- [Estrutura do projeto](#-estrutura-do-projeto)
- [Como rodar o projeto](#-como-rodar-o-projeto)
- [Scripts disponíveis](#-scripts-disponíveis)
- [Integração com a API](#-integração-com-a-api)
- [Autenticação e sessão](#-autenticação-e-sessão)
- [Rotas e perfis de acesso](#-rotas-e-perfis-de-acesso)
- [Conceitos praticados](#-conceitos-praticados)
- [Status do projeto](#-status-do-projeto)
- [Autor](#-autor)

---

## ✨ Funcionalidades

- **Autenticação:**
  - Cadastro de usuários com validação dos dados (nome, e-mail, senha e confirmação de senha).
  - Login com e-mail e senha, retornando um token JWT da API.
  - Sessão persistida no `localStorage` — o usuário continua logado ao recarregar a página.
  - Logout pelo ícone de sair no cabeçalho, que também exibe o nome do usuário logado.
- **Solicitação de reembolso (funcionário):**
  - Formulário com nome da solicitação, categoria, valor e upload de comprovante.
  - O comprovante é enviado para a API e, em seguida, a solicitação é registrada com o nome do arquivo salvo.
  - Valores aceitam vírgula ou ponto como separador decimal.
  - Tela de confirmação após o envio (acessível apenas a partir do formulário).
- **Painel de solicitações (gestor):**
  - Listagem das solicitações vindas da API, com nome do funcionário, descrição, ícone da categoria e valor formatado em Real (BRL).
  - Busca por nome.
  - Paginação feita no servidor (5 itens por página).
  - Visualização dos detalhes de uma solicitação em modo somente leitura, com link para abrir o comprovante.
- **Validação de formulários** com Zod e exibição das mensagens de erro da validação ou da API.
- **Categorias de despesa:** Alimentação, Hospedagem, Transporte, Serviços e Outros.
- **Roteamento por perfil:** as rotas disponíveis mudam conforme o papel do usuário logado (`employee`, `manager` ou não autenticado).
- **Componentes de feedback:** tela de carregamento (`Loading`) enquanto a sessão é restaurada e página 404 (`NotFound`).
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
| [Axios](https://axios-http.com/) | Cliente HTTP para comunicação com a API |
| [Zod](https://zod.dev/) | Validação dos dados dos formulários |
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
│   │   ├── Header.tsx      # Cabeçalho com nome do usuário e logout
│   │   ├── Input.tsx       # Campo de texto com legenda
│   │   ├── Loading.tsx     # Indicador de carregamento
│   │   ├── Pagination.tsx  # Controles de paginação
│   │   ├── RefundItem.tsx  # Item da listagem de reembolsos
│   │   ├── Select.tsx      # Campo de seleção com legenda
│   │   └── Upload.tsx      # Campo de upload de comprovante
│   ├── contexts/
│   │   └── AuthContext.tsx # Contexto de autenticação (sessão, login, logout)
│   ├── dtos/               # Tipos globais das respostas da API
│   │   ├── categories.d.ts     # Categorias aceitas pela API
│   │   ├── refund.d.ts         # Reembolso e listagem paginada
│   │   └── user.d.ts           # Usuário, perfil (role) e token
│   ├── hooks/
│   │   └── useAuth.tsx     # Hook de acesso ao AuthContext
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
│   ├── services/
│   │   └── api.ts          # Instância do Axios com a URL base da API
│   ├── utils/              # Funções e constantes auxiliares
│   │   ├── categories.ts       # Categorias de despesa e seus ícones
│   │   ├── classMerge.ts       # Helper que combina clsx + tailwind-merge
│   │   └── formatCurrency.ts   # Formatação de valores em BRL
│   ├── App.tsx             # Componente raiz (envolve as rotas com o AuthProvider)
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
- **API back-end do Refund** rodando em `http://localhost:3333`

### Passo a passo

```bash
# 1. Clone o repositório
git clone <url-do-repositorio>

# 2. Acesse a pasta do projeto
cd web-refund

# 3. Instale as dependências
npm install

# 4. Com a API já em execução, inicie o servidor de desenvolvimento
npm run dev
```

A aplicação ficará disponível em **http://localhost:5173**.

> ⚠️ Sem a API rodando, as telas abrem normalmente, mas login, cadastro, envio e listagem de solicitações vão falhar.

---

## 📜 Scripts disponíveis

| Comando | Descrição |
| --- | --- |
| `npm run dev` | Inicia o servidor de desenvolvimento com hot reload |
| `npm run build` | Verifica os tipos com o TypeScript e gera o build de produção em `dist/` |
| `npm run preview` | Serve localmente o build de produção |

---

## 🔌 Integração com a API

A comunicação é feita por uma instância do Axios definida em [`src/services/api.ts`](src/services/api.ts), com URL base `http://localhost:3333`. Para apontar para outro endereço, altere o `baseURL` nesse arquivo (e o link do comprovante em [`src/pages/Refund.tsx`](src/pages/Refund.tsx), que também usa esse endereço).

Endpoints consumidos pelo front-end:

| Método | Endpoint | Usado em | Descrição |
| --- | --- | --- | --- |
| `POST` | `/users` | Cadastro | Cria um novo usuário |
| `POST` | `/sessions` | Login | Autentica e retorna `token` e dados do `user` |
| `POST` | `/uploads` | Nova solicitação | Envia o comprovante (`multipart/form-data`) e retorna o `filename` |
| `POST` | `/refunds` | Nova solicitação | Cria a solicitação (`name`, `category`, `amount`, `filename`) |
| `GET` | `/refunds?name=&page=&perPage=` | Painel | Lista solicitações com busca e paginação |
| `GET` | `/refunds/:id` | Detalhes | Retorna os dados de uma solicitação |
| `GET` | `/uploads/:filename` | Detalhes | Abre o arquivo do comprovante |

Os formatos das respostas estão tipados globalmente em [`src/dtos/`](src/dtos/) (`UserAPIResponse`, `RefundAPIResponse`, `RefundsPaginationAPIResponse`, `CategoriesAPIEnum`).

---

## 🔐 Autenticação e sessão

A sessão é gerenciada pelo [`AuthContext`](src/contexts/AuthContext.tsx) e acessada nos componentes pelo hook [`useAuth`](src/hooks/useAuth.tsx):

- **`save(data)`** — chamado após o login: guarda o usuário e o token no `localStorage` (chaves `@refund:user` e `@refund:token`) e define o cabeçalho `Authorization: Bearer <token>` como padrão em todas as requisições.
- **Restauração** — ao abrir a aplicação, a sessão é recuperada do `localStorage`; enquanto isso, a tela de `Loading` é exibida.
- **`remove()`** — faz o logout: limpa a sessão e o `localStorage` e redireciona para a tela inicial.

---

## 🧭 Rotas e perfis de acesso

As rotas são escolhidas em [`src/routes/index.tsx`](src/routes/index.tsx) com base no papel (`role`) do usuário autenticado, retornado pela API:

| Perfil | Rota | Página |
| --- | --- | --- |
| Não autenticado | `/` | Login |
| Não autenticado | `/signup` | Cadastro |
| Funcionário (`employee`) | `/` | Nova solicitação de reembolso |
| Funcionário (`employee`) | `/confirm` | Confirmação de envio |
| Gestor (`manager`) | `/` | Painel de solicitações |
| Gestor (`manager`) | `/refund/:id` | Detalhes de uma solicitação |
| Todos | `*` | Página não encontrada (404) |

---

## 🧠 Conceitos praticados

- Componentização e reaproveitamento de componentes
- Tipagem de props com TypeScript, estendendo props nativas (`React.ComponentProps<"button">`)
- Tipos globais (`.d.ts`) para as respostas da API
- Componentes com variantes de estilo
- Gerenciamento de estado com `useState` e efeitos com `useEffect`
- Context API (`createContext` + `use`) e hook customizado (`useAuth`)
- Persistência de sessão com `localStorage`
- Formulários controlados e formulários com `useActionState` (React 19)
- Validação de dados com Zod (incluindo `refine` e `coerce`)
- Consumo de API REST com Axios, cabeçalho de autorização padrão e tratamento de erros (`AxiosError`, `ZodError`)
- Upload de arquivos com `FormData`
- Paginação e busca feitas no servidor
- Roteamento com rotas aninhadas, layouts (`<Outlet />`), parâmetros (`useParams`) e navegação programática (`useNavigate`)
- Passagem de estado entre rotas (`location.state`) e redirecionamento com `<Navigate />`
- Separação de rotas por perfil de usuário
- Tema customizado no Tailwind CSS 4 com `@theme` (cores, fonte e tamanhos)
- Mesclagem de classes com `clsx` + `tailwind-merge`
- Formatação de moeda com `Intl.NumberFormat`
- Layout responsivo

---

## 🚧 Status do projeto

Em desenvolvimento. O front-end já está integrado à API para autenticação, cadastro, envio de comprovantes e gerenciamento das solicitações.

- [x] Integração com uma API back-end
- [x] Autenticação real e contexto de sessão
- [x] Envio e armazenamento dos comprovantes
- [x] Validação de formulários
- [ ] URL da API configurável por variável de ambiente
- [ ] Testes automatizados

---

## 👨‍💻 Autor

Desenvolvido por **ViniciusLegacy** para fins de estudo.
