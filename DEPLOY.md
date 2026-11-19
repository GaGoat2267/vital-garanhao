# Colocar o site no ar (GitHub + PythonAnywhere)

O GitHub guarda o código. Quem **roda** o site é o PythonAnywhere, que puxa o código do GitHub.
São uns 10 minutos, e a maior parte é um script que já faz o trabalho pesado.

## 1. Criar a conta

Crie a conta gratuita em https://www.pythonanywhere.com. O seu nome de usuário vira o endereço
do site: `SEUUSUARIO.pythonanywhere.com`.

## 2. Criar o site (3 cliques)

Aba **Web** → **Add a new web app** → **Next** → **Manual configuration** → escolha **Python 3.13** → **Next**.

## 3. Instalar o projeto (1 comando)

Aba **Consoles** → **Bash**. Cole as linhas abaixo, uma de cada vez:

```
git clone https://github.com/GaGoat2267/vital-garanhao.git
cd vital-garanhao
bash setup_pythonanywhere.sh
```

O script cria o ambiente Python, instala o Flask, cria o banco e escreve o arquivo de
configuração do servidor. No final ele mostra o que falta.

## 4. Terminar na aba Web (3 itens)

1. Em **Virtualenv**, digite o caminho que o script mostrou (algo como `/home/SEUUSUARIO/venv-vital`).
2. Ligue **Force HTTPS**.
3. Clique no botão verde **Reload**.

Abra `https://SEUUSUARIO.pythonanywhere.com`. Use sempre `https://`: com `http://` o login não
fica salvo.

## 5. Conta de demonstração (opcional, recomendado)

Para o site já aparecer cheio de dados de exemplo, no mesmo Bash console:

```
cd ~/vital-garanhao
~/venv-vital/bin/python seed_demo.py professor SUASENHA
```

Troque `professor` e `SUASENHA` pelo usuário e senha que você quiser (senha com 8+ caracteres).
Os dados são fictícios.

## 6. Antes de apresentar

- Na aba Web, clique no botão de **estender o prazo** do site (o gratuito expira se não renovar).
  Faça isso um ou dois dias antes.
- Abra o site e faça login uma vez para conferir.
- Não use dados financeiros reais: qualquer pessoa com o link pode criar conta.

## Atualizar depois de mudar o código

1. Envie o arquivo novo para o GitHub.
2. No Bash console: `cd ~/vital-garanhao && git pull`
3. Se o `requirements.txt` mudou: `~/venv-vital/bin/pip install -r requirements.txt`
4. Aba Web → **Reload**.

O `vital.db` (usuários e dados) fica só no servidor e não é afetado pelo `git pull`.

## Se der erro

- Página de erro (502/504): na aba Web, abra o **Error log** e leia as últimas linhas.
- Login não "fica": confira se está usando `https://` e se **Force HTTPS** está ligado.
- O script não achou o arquivo WSGI: ele imprime o texto na tela. Na aba Web, abra o link do
  arquivo WSGI, apague tudo e cole esse texto.
- A versão do Python do **Virtualenv** deve ser a mesma escolhida no passo 2.

## Cotações ao vivo (opcional)

Sem configurar nada, a brapi.dev cota só alguns ativos (PETR4, VALE3, ITUB4, MGLU3). Para cotar todos:

1. Crie uma conta grátis em https://brapi.dev e copie o seu token.
2. Aba **Web** → seção **Code** → abra o arquivo WSGI e acrescente, antes da última linha, `os.environ['BRAPI_TOKEN'] = 'SEU_TOKEN'`.
3. Clique em **Reload**.

## Assistente de IA (opcional)

A aba **Assistente** usa a API da Anthropic. Sem chave, ela avisa que não está configurada e o resto do site funciona normalmente.

1. Crie uma conta em https://console.anthropic.com, adicione crédito e gere uma chave de API (começa com `sk-ant-`).
2. Aba **Web** do PythonAnywhere → abra o arquivo WSGI e acrescente, antes da última linha: `os.environ['ANTHROPIC_API_KEY'] = 'SUA_CHAVE'`
3. Clique em **Reload**.

Cuidados:
- **Nunca** coloque a chave no GitHub nem em arquivos do projeto. Ela só vale no arquivo WSGI do servidor.
- Cada pergunta custa um pouco. O app limita cada usuário a 20 perguntas por hora e 80 por dia. Veja o gasto no console da Anthropic.
- O modelo padrão é `claude-haiku-5-5`, que é o mais barato. Para trocar, defina `AI_MODEL` do mesmo jeito.
- A conta gratuita do PythonAnywhere só acessa sites de uma lista. Se o assistente responder "não conseguiu responder agora" mesmo com a chave certa, confira se `api.anthropic.com` está em https://www.pythonanywhere.com/whitelist/.
