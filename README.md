<p align="center">
  <img width="1536" height="1024" alt="revolkov vault" src="https://github.com/user-attachments/assets/ca85166c-7c0c-4dc4-8685-86630d8c920d" />
</p>

<h1 align="center">REVOLKOV VAULT</h1>

<p align="center">
  <b>Pague uma vez. Suas senhas são suas para sempre.</b><br>
  Cofre de senhas local para Windows, com extensão para navegador. Sem conta, sem servidor, sem nuvem, sem mensalidade.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/versão-1.10.0-8B5CF6" alt="Versão 1.10.0">
  <img src="https://img.shields.io/badge/Windows-10%20%7C%2011-0078D6?logo=windows&logoColor=white" alt="Windows 10 e 11">
  <img src="https://img.shields.io/badge/Java-21-ED8B00?logo=openjdk&logoColor=white" alt="Java 21">
  <img src="https://img.shields.io/badge/Argon2id%20%2B%20AES--256--GCM-22C55E" alt="Argon2id + AES-256-GCM">
  <img src="https://img.shields.io/badge/testes-268-2EA44F" alt="268 testes automatizados">
</p>

<p align="center">
  <a href="#download">Baixar versão de avaliação</a> ·
  <a href="#funcionalidades">Funcionalidades</a> ·
  <a href="#segurança">Segurança</a> ·
  <a href="#como-o-projeto-é-testado">Testes</a> ·
  <a href="#contato">Contato</a>
</p>

<!-- Troque pela captura da tela principal (cofre aberto, com dados de TESTE, nunca os reais). -->
<p align="center">
  <img src="docs/screenshots/cofre.png" width="820" alt="Tela principal do Revolkov Vault com a lista de credenciais">
</p>

---

## Por que o Revolkov Vault existe

Testei vários gerenciadores de senhas e sempre esbarrava no mesmo problema: **assinatura mensal ou anual**. Se eu deixasse de pagar, perdia o acesso às minhas próprias senhas e ficava dependente da plataforma de outra pessoa.

O Revolkov Vault nasceu com três promessas:

- **Licença única e vitalícia.** Você paga uma vez e usa para sempre.
- **Tudo fica no seu computador.** Não existe conta online, servidor ou sincronização com nuvem de terceiros.
- **Você nunca perde o acesso aos seus dados.** Nenhuma licença, prazo ou servidor fora do ar impede você de abrir ou exportar as suas senhas.

> Este repositório é a vitrine do projeto. O código-fonte é privado, porque o Revolkov Vault está sendo preparado para venda.

---

## Funcionalidades

<!-- Uma captura para cada bloco ajuda muito: gerador, página Segurança, extensão. -->

**Cofre**
- Credenciais com serviço, conta, senha, site, notas, etiquetas, favoritas e campos personalizados (PIN, número do cliente, com opção de campo oculto).
- Notas seguras, histórico das 10 últimas senhas de cada credencial e lixeira com 30 dias para restaurar.
- Busca, filtros por favoritas, notas ou etiqueta, e ordenação.

**Extensão para navegador** (Chrome, Edge, Opera GX e Firefox)
- Lista as credenciais do site aberto e preenche usuário e senha com um clique, pelo atalho **Alt+Shift+L** ou pelo botão direito.
- Conversa só com o app instalado no computador. Nenhuma senha passa pela internet nem fica guardada na extensão.
- Pareamento com código de 6 dígitos aprovado no app e canal cifrado de ponta a ponta.
- Só oferece a credencial no site certo. Endereços parecidos, usados em golpes de phishing, nunca recebem nada.
- **Já aprovada na loja de complementos do Firefox.**

**Códigos de verificação em duas etapas (TOTP)**
- Gera o código a partir da chave secreta ou do link `otpauth://` do site (RFC 6238).

**Gerador e medidor de senhas**
- Senhas de 4 a 64 caracteres ou frases-senha, sempre com `SecureRandom`.
- Medidor de força que reconhece senhas comuns, repetições, sequências e padrões de teclado.

**Segurança da sessão**
- Desbloqueio opcional com **Windows Hello** (PIN, rosto ou digital).
- Bloqueio automático por inatividade e bloqueio manual (Ctrl+L).
- Espera crescente entre tentativas erradas de desbloqueio, de 30 segundos até 1 hora.
- A área de transferência é limpa automaticamente e a senha copiada fica fora do histórico do Windows (Win+V).

**Recuperação e backups**
- **Frase de recuperação** de 10 palavras para abrir o cofre se a senha mestre for esquecida.
- Backups criptografados verificados depois de gravar, com backup automático opcional.

**Saúde das senhas**
- Página Segurança com senhas vazadas, repetidas e fracas.
- Verificação de vazamentos no Have I Been Pwned, desligada por padrão.

**Importar e exportar**
- Importação de Bitwarden, KeePassXC, 1Password, LastPass, Proton Pass, Chrome/Edge e Firefox.
- Exportação em CSV a qualquer momento.

**Interface**
- Tema claro e escuro, em português, inglês e espanhol.
- Navegação completa por teclado e nomes acessíveis para leitores de tela.

---

## Segurança

### Criptografia

| Item | Como é feito |
|---|---|
| Derivação de chave | **Argon2id** (64 MiB, 3 passagens). Cofres criados com PBKDF2 em versões antigas são migrados automaticamente. |
| Conteúdo do cofre | **AES-256-GCM**, criptografia autenticada: senha errada ou arquivo adulterado são detectados. |
| Chave do cofre | Uma chave aleatória cifra o conteúdo e é guardada protegida pela senha mestre e, se configurada, pela frase de recuperação. Trocar a senha mestre não exige recifrar tudo. |
| Windows Hello | Uma chave presa ao TPM do computador libera uma cópia protegida da chave do cofre. Backups não levam esse acesso. |
| Extensão ↔ app | Troca de chaves ECDH P-256 com compromisso prévio, HKDF-SHA256 e AES-256-GCM com número de sequência, que impede repetição e adulteração de mensagens. |

### Privacidade

- **Nenhum dado do cofre sai do computador.** Não há telemetria.
- A verificação de vazamentos usa **k-anonimato**: só os 5 primeiros caracteres do hash da senha são consultados, e a comparação acontece no seu computador.
- A extensão não coleta, não envia e não guarda dados.

### Limitações conhecidas

Um gerenciador de senhas precisa ser honesto sobre o que não faz:

- Se você esquecer a senha mestre **e** não tiver configurado a frase de recuperação, ninguém consegue abrir o cofre, nem o desenvolvedor.
- Com o cofre desbloqueado, as senhas ficam descriptografadas na memória do programa, como em qualquer gerenciador local. Um programa malicioso rodando no seu usuário do Windows pode, no pior caso, ler essa memória.
- O instalador ainda **não tem assinatura digital**, então o Windows SmartScreen mostra "aplicativo não reconhecido".
- O código ainda não passou por auditoria de segurança independente.

---

## Como o projeto é testado

**Testes automatizados:** 268 testes (JUnit 5) cobrem criptografia, formato do arquivo, migração de versões antigas, backup e restauração, importação, TOTP, pareamento e protocolo da extensão, correspondência de sites e simulações de ataque (mensagens repetidas, adulteradas e extensões desconhecidas). Os testes rodam a cada alteração pelo GitHub Actions e nunca tocam no cofre real.

**Testadores reais:** desde as primeiras versões, o app é distribuído para um grupo de testadores, com dois perfis:
- **Usuários sem conhecimento técnico**, que avaliam a experiência de uso. Exemplos de melhorias que vieram deles: as senhas geradas passaram de 12 para 14 caracteres por padrão, e o app ganhou opção de idioma.
- **Profissionais de QA**, que avaliam a segurança. A migração para Argon2id, a espera crescente entre tentativas erradas, as favoritas e a própria extensão para navegador vieram dessas conversas.

As novas versões vão primeiro para quem devolve feedback, o que mantém o ciclo de melhorias ativo.

---

## Download

<!-- Suba o instalador gerado pelo package-trial.ps1 na página de Releases DESTE repositório e troque o link abaixo. -->

**[Baixar a versão de avaliação para Windows](../../releases/latest)**

- A versão de avaliação funciona por um período limitado a partir da primeira execução.
- **Quando o prazo termina, os seus dados continuam seus:** o app permite salvar uma cópia de tudo antes de fechar.
- Como o instalador ainda não é assinado, o SmartScreen vai avisar: clique em **Mais informações → Executar assim mesmo**.

A extensão para Firefox está disponível na loja de complementos da Mozilla. <!-- Coloque aqui o link do AMO. --> As versões para Chrome, Edge e Opera GX estão a caminho.

---

## Tecnologias

Java 21 · JavaFX 21 · Maven · Bouncy Castle (Argon2id) · JUnit 5 · GitHub Actions · jpackage + WiX (instalador) · C# (ponte do Windows Hello) · JavaScript e WebExtensions Manifest V3 (extensão) · Native Messaging

---

## Roadmap

- [x] Argon2id + AES-256-GCM, frase de recuperação e troca de senha mestre
- [x] Backups verificados e backup automático
- [x] 2FA, lixeira, histórico, etiquetas, campos personalizados e notas seguras
- [x] Desbloqueio com Windows Hello
- [x] Extensão para navegador (Firefox publicado)
- [ ] Publicação da extensão na Chrome Web Store e na Edge Add-ons
- [ ] Assinatura digital do instalador
- [ ] Licença vitalícia e primeira versão comercial
- [ ] Versão para celular

---

## Contato

Desenvolvido por **Alexander Revoredo**, estudante de Engenharia de Software.

- Portfólio: [portifolio-two-delta-76.vercel.app](https://portifolio-two-delta-76.vercel.app/)
- LinkedIn: [linkedin.com/in/alex-revolkov](https://www.linkedin.com/in/alex-revolkov/)
- E-mail: alexanderrevoredo@gmail.com

Recrutadores e avaliadores técnicos: posso apresentar o código e a arquitetura numa chamada. É só entrar em contato.

<sub>© 2026 Alexander Revoredo. Todos os direitos reservados.</sub>
