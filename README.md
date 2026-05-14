# 📡 Comunicação SCI – TMS320F28379D + Python

Projeto de comunicação serial entre o microcontrolador **TMS320F28379D** e um computador via protocolo SCI. 
Com o novo **Code Composer Studio (CCS) 20.5 (Theia)**, tanto o **firmware em C** quanto a **interface de controle em Python** podem ser gerenciados e executados diretamente no mesmo ambiente de desenvolvimento.

Este projeto permite o **envio** e a **recepção** de inteiros (`int16_t`) entre o DSP e o PC.

---

## 🗂️ Estrutura do Projeto

```text
sci_python/
├── images/             # Imagens e documentação visual
├── microcontroller/    # Projeto C para o TMS320F28379D (CCS)
│   ├── main.c          # Código principal com interrupções SCI
│   └── sci.syscfg      # Configuração de pinos e periféricos
├── python/             # Código Python para controle no PC
│   └── main.py         # Script de interface serial
└── README.md

```

---

## 🧰 Requisitos

### PC e IDE

* **Code Composer Studio (CCS) 20.5 ou superior (Theia)**.
* **Python 3.11+** instalado e configurado no PATH do sistema.
* Driver da porta COM (XDS100v2/v3, geralmente instalado com o CCS).

### Hardware

* Launchpad **TMS320F28379D**.
* Cabo USB para conexão e depuração.

---

## 🔧 Passo a Passo: Preparação

### 1. Obter o Projeto (📌 Fork e Clone)

1. Faça o **Fork** do repositório: [`https://github.com/Pguilhermem/sci_python`](https://github.com/Pguilhermem/sci_python).
2. Abra o **CCS 20.5 (Theia)**.
3. Use a funcionalidade de Git integrada:
* Vá na aba **Source Control** (ícone de ramificação na lateral esquerda).
* Clique em **Clone Repository**.
* Insira a URL do seu fork e escolha a pasta local.


4. Após o clone, adicione a pasta ao workspace se necessário: `File > Add Folder to Workspace...`.

---

## ⚙️ Parte 1 – Firmware (C/C++)

1. No Explorer do CCS, expanda a pasta `microcontroller`.
2. Compile e rode o projeto com **F5**.

---

## 🖥️ Parte 2 – Interface Python (Execução no CCS Theia)

O CCS Theia permite rodar Python no terminal integrado.

### 1. Configuração do Ambiente Virtual (Venv)

Abra o terminal (`Terminal > New Terminal`) e execute:

```bash
cd python
python -m venv .venv

```

**Ative o ambiente:**

* **Windows:** `.\\.venv\\Scripts\\activate`
* **Linux/macOS:** `source .venv/bin/activate`

### 2. Instalação de Dependências

Com a `.venv` ativa, instale o `pyserial`:

```bash
pip install pyserial

```

### 3. Seleção do Interpretador

Para evitar erros de edição, abra o `main.py`, clique na versão do Python na barra de status (canto inferior direito) e selecione o interpretador que está dentro da pasta `.venv`.

---

## ⚠️ Problemas Comuns ao Executar Python

Caso encontre erros ao rodar o script, verifique os pontos abaixo:

### 1. `ModuleNotFoundError: No module named 'serial'`

* **Causa:** O script está tentando rodar sem o ambiente virtual ativo ou o `pyserial` não foi instalado na `.venv`.
* **Solução:** Certifique-se de que o prefixo `(.venv)` aparece no terminal. Se não, ative o ambiente e rode `pip install pyserial` novamente.

### 2. `PermissionError: [Errno 13] could not open port`

* **Causa:** A porta COM já está aberta por outro programa.
* **Solução:** Verifique se o **Terminal Serial** do próprio CCS ou outro software (como PuTTY) está conectado à mesma porta. Feche a conexão nesses programas antes de rodar o Python.

### 3. `FileNotFoundError: [Errno 2] could not open port 'COMX'`

* **Causa:** O número da porta serial no código (`SERIAL_PORT`) está incorreto.
* **Solução:** Abra o **Gerenciador de Dispositivos** (Windows), verifique em qual porta COM o "XDS100v2" está enumerado e atualize a variável no `main.py`.

### 4. Erro de Script no PowerShell (Windows)

* **Causa:** Políticas de execução do Windows impedem a ativação da `.venv`.
* **Solução:** No terminal como Administrador, execute: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`.

### 5. Timeout ou Falha na Resposta

* **Causa:** O DSP está pausado no Debugger.
* **Solução:** Verifique se você clicou em **Continue** no CCS. Se o programa estiver parado em um breakpoint, o Python não receberá dados.

---

## 📄 Licença

Este projeto é livre para fins educacionais.