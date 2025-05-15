# Nodexn - Aplicativo de Conversão e Execução de Projetos Node.js para .EXN

## 🌟 Visão Geral
O Nodexn é um aplicativo pronto para uso que permite converter e executar projetos Node.js CLI em um ambiente isolado, sem necessidade de instalação ou configuração prévia.

## 📥 Como Obter
1. **Download**:
   - Baixe o apk mais recente em [releases](https://github.com/ON00dev/Nodexn/releases)

2. **Execução**:
   - Abra o app `Nodexn`

## 📱 Interface do Usuário

### Áreas Principais:
1. **Seletor de Arquivos**:
   - Botão para selecionar arquivos/projetos
   - Visualização dos arquivos selecionados

2. **Painel de Controle**:
   - "Converter para EXN" - Transforma seu projeto em um arquivo .exn
   - "Executar EXN" - Roda arquivos .exn no ambiente seguro

3. **Saída**:
   - Exibe logs e resultados da execução
   - Destaque para mensagens de erro

## 🛠️ Funcionalidades Principais

### Conversão de Projetos
1. Selecione a pasta do seu projeto Node.js
2. Clique em "Converter"
3. Receba um arquivo `.exn` único contendo:
   - Todos os arquivos do projeto
   - Dependências incluídas
   - Metadados e configurações

### Execução Segura
- Ambiente isolado (sandbox)
- Controle de permissões
- Suporte a:
  - [x] CommonJS
  - [x] ES Modules
  - [x] Plugins (.exnplugin.js)

## ⚙️ Configuração

### Permissões
Edite o `package.json` do seu projeto para definir permissões e declarar o tipo de módulos:
```json
{
  "type": "module",
  "exnPermissions": [
    "filesystem",
    "network"
  ]
}
```
Em `type` coloque `"module"` para módulos `ES modules`. Ou `"commonjs"` para módulos `CommonJS`.

## Mapeamento de Permissões por Módulo

### Módulos Nativos do Node.js

| Módulo               | Permissão EXN              | Alternativa Restrita       | Descrição                     |
|----------------------|---------------------------|---------------------------|-------------------------------|
| `fs`                 | `"filesystem"`            | `"filesystem:read"`       | Acesso completo ao sistema de arquivos |
| `child_process`      | `"process"`               | -                         | Execução de processos filhos  |
| `crypto`             | `"crypto"`                | `"crypto:hash"`           | Operações criptográficas      |
| `http`/`https`       | `"network"`               | `"network:http"`          | Comunicação HTTP(S)           |
| `net`                | `"full_network"`          | `"network:tcp"`           | Comunicação TCP               |
| `dgram`              | `"full_network"`          | `"network:udp"`           | Comunicação UDP               |
| `os`                 | `"system_info"`           | -                         | Informações do sistema        |
| `cluster`            | `"process"`               | -                         | Criação de clusters           |
| `readline`           | `"stdio"`                 | -                         | Interface de linha de comando |
| `worker_threads`     | `"threads"`               | -                         | Threads de trabalho           |

### Módulos de Terceiros Comuns

| Módulo/Pacote        | Permissão EXN              | Observações                |
|----------------------|---------------------------|----------------------------|
| `hyperswarm`         | `"full_network"`          | Requer UDP+TCP             |
| `express`            | `"network"`               | Depende de `http`          |
| `socket.io`          | `"full_network"`          | Usa WebSockets + polling   |
| `level`/`leveldb`    | `"filesystem"`            | Acesso a banco de dados    |
| `sqlite3`            | `"filesystem"`+`"native"` | Requer módulos nativos     |
| `sharp`              | `"native"`+`"filesystem"` | Processamento de imagens   |

### Permissões Especiais

| Permissão            | Módulos Relacionados       | Escopo                     |
|----------------------|---------------------------|----------------------------|
| `"native"`           | Módulos compilados        | `sqlite3`, `bcrypt`, etc.  |
| `"environment"`      | `process.env`             | Variáveis de ambiente      |
| `"clipboard"`        | Pacotes de clipboard      | Acesso à área de transferência |
| `"timers"`           | `setInterval`, `cron`     | Operações agendadas        |

Veja sobre boas práticas e recomendações de permissões em [Regras de Permissões](#-regras-de-permissões)

## Plugins
Crie arquivos `.exnplugin.js` para estender funcionalidades:
```javascript
plugins.register('meu-plugin', {
  init(sandbox) {
    sandbox.minhasFuncoes = {
      saudacao: () => "Olá do plugin!"
    };
  }
});
```

## 📌 Dicas Rápidas

1. **Para projetos simples**:
   - Basta ter um `main.js` ou `index.js` na raiz

2. **Para projetos complexos**:
   - Mantenha a estrutura padrão (src/, public/, etc.)
   - Especifique o ponto de entrada no `package.json` no campo `main`

3. **Para distribuição**:
   - Converta para .exn
   - Compartilhe o único arquivo gerado

## 📑 Regras de Permissões

1. **Herança**:
   - Se um módulo requer `"full_network"`, ele automaticamente inclui `"network"`
   
2. **Combinações**:
   - Módulos como `sqlite3` precisam de múltiplas permissões (`"filesystem"` + `"native"`)

3. **Erros Comuns**:
   
   Exemplo:
   ```bash
   # Erro típico se faltar permissão
   PermissionDenied: Module 'fs' requires 'filesystem' permission
   ```

5. **Boas Práticas**:
   
   Exemplo:
   ```json
   // SEMPRE especifique o mínimo necessário
   "exnPermissions": [
     "network:http",     // Apenas HTTP
     "filesystem:read"   // Apenas leitura
   ]
   ```

## ⚠️ Limitações Conhecidas

- Tamanho máximo do projeto: 500MB
- Não suporta módulos nativos do Node.js
- Algumas APIs restritas por segurança

## 🤔 Perguntas Frequentes

**P: Posso usar npm install no projeto convertido?**  
R: Não é necessário - todas as dependências são incluídas no arquivo .exn

**P: Como debugar erros?**  
R: Verifique o painel de saída no aplicativo e os logs gerados

**P: Meu plugin não está funcionando**  
R: Verifique se:
1. O arquivo termina com .exnplugin.js
2. Está na pasta raiz ou /plugins
3. Usa plugins.register() corretamente

---

📌 **Nota**: Este é um aplicativo autônomo - não requer Node.js instalado mas requer acesso à internet para funcionar!
