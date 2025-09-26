# Sobre criptografia!

### 1. Criptografia
-   **Simétrica**: mesma chave para criptografar e descriptografar. Exemplo: AES.
-   **Assimétrica**: par de chaves (pública e privada).
    -   **Pública** → usada para criptografar ou validar assinaturas.
    -   **Privada** → usada para descriptografar ou assinar.

----------

### 2. Hash

-   **Mão única**: não dá para “desfazer” o cálculo.
-   **Determinístico**: mesma entrada → mesma saída.
-   **Avalanche**: mínima mudança → resultado totalmente diferente.  

Senhas não devem ser guardadas em texto claro, mas sim com hash + salt, **salt** e **Pepper** : **Salt** → é um valor aleatório (gerado normalmente pelo sistema) que é **concatenado à senha antes do hash**.    

## Exemplo: 
-	Senha do usuário: `senha123` 
-   Salt gerado: `Xy9!`  
-	Entrada real para o hash: `senha123Xy9!`  
-   **Hash final**: `SHA256(senha123Xy9!)`
       
👉 Isso faz com que senhas iguais tenham hashes **diferentes**, quebrando ataques de **rainbow tables** (que dependem de pré-computar hashes de senhas comuns).
    
-   **Pepper** → é parecido, mas em vez de ser salvo junto da senha, ele é um valor **secreto, global e fixo**, guardado separadamente (ex.: em variável de ambiente do servidor).
    
## Exemplo:  
-   Senha do usuário: `senha123`  
-   Pepper secreto: `#SuperSegredo$`  
-   Entrada real: `senha123#SuperSegredo$`  
-   Hash final: `SHA256(senha123#SuperSegredo$)`
        
👉 Assim, mesmo que o banco de dados com os hashes e os salts seja vazado, o atacante ainda não tem o pepper.
    
📌 Juntos, salt + pepper deixam o trabalho do invasor **muito mais caro**.

----------

### 3. Codificação × Criptografia

-   **Codificação**: transforma dados em outro formato, mas não protege (ex.: Base64, ASCII).    
-   **Criptografia**: protege dados de leitura indevida.  
    👉 Cai muito em prova prática do tipo: “Dado um texto em Base64, diga se está criptografado ou apenas codificado.”
    
----------

### 4. Esteganografia

-   Esconde informação dentro de outro arquivo (imagem, áudio, vídeo).  
    Exemplo: um `.jpg` que parece inocente, mas esconde uma mensagem.
    
----------

### 5. GPG4Win / Cleopatra

-   **Gerar par de chaves** (pública + privada).
-   **Exportar chave pública** (para alguém poder criptografar algo para você).
-   **Importar chave pública** (de outra pessoa).
-   **Criptografar** um arquivo/mensagem para alguém.
-   **Assinar** um arquivo (para garantir autenticidade).
-   **Verificar assinatura** com a chave pública da outra pessoa.
----------
#### 1) Gerar par de chaves (pública + privada)

1. Abra o **Kleopatra**.
2. Menu **File → New Certificate**.
3. Escolha **Create a personal OpenPGP key pair** e clique **Next**.
4. Preencha **Nome** e **Email** (opcional comentário). Clique **Next**.
5. (Opcional) clique em **Advanced Settings** para escolher tipo RSA/RSA e **4096 bits** e definir validade (recomendo 1 ano ou sem expiração se souber gerenciar). Clique **OK**.
6. Clique **Create** e escolha uma **passphrase** forte quando solicitado.
7. Ao finalizar, seja obrigado a **backup**: salve o **revocation certificate** (crie e guarde em local seguro) — use-o se precisar revogar a chave.

#### 2) Exportar chave pública (para alguém poder criptografar pra você)

1. Na lista de certificados, selecione sua chave.
2. Clique com o botão direito → **Export Certificates...** (ou botão **Export**).
3. Escolha formato (ASCII-armored `.asc` é recomendado) e salve o arquivo. Envie esse `.asc` à pessoa.

#### 3) Importar chave pública (de outra pessoa)

1. Recebeu `outra_chave.asc`? Abra Kleopatra → **File → Import Certificates...** → selecione o arquivo `.asc`.
2. A chave aparecerá na lista; revise o **fingerprint** e confirme que é a chave correta (compare por outro canal se for crítico).
3. Opcional: defina **Trust**/ownertrust se você conhece e confia na pessoa (botão direito → **Set Ownertrust**).

#### 4) Criptografar um arquivo/mensagem para alguém

> A criptografia usa a **chave pública do destinatário** — você não precisa da chave privada dele.

1. No Explorer, clique com o botão direito no arquivo → **Sign and Encrypt** (instalador Gpg4win adiciona essas opções).
2. No diálogo, escolha **Encrypt** e selecione o(s) destinatário(s) (a chave pública da outra pessoa).
3. Escolha se quer **Sign** também (opcional) e confirme. Será criado um arquivo `.gpg` ou `.asc`.

## Kleopatra — texto/clipboard

1. Abra Kleopatra → menu **Clipboard → Encrypt** (ou use o botão correspondente).
2. Cole a mensagem, escolha destinatários e cifre. Copie o resultado cifrado para enviar.

#### 5) Assinar um arquivo (para garantir autenticidade)
Há duas formas: assinatura embutida (clear/sign) ou assinatura destacada (detached).

1. Clique com o botão direito no arquivo → **Sign/Encrypt**.
2. Marque **Sign** (pode escolher detached signature se quiser apenas `.sig`).
3. Escolha sua chave para assinar e confirme. Resultado: arquivo assinado (ou `.sig` se detached).

#### 6) Verificar assinatura com a chave pública da outra pessoa
1. Se você recebeu `arquivo` + `arquivo.sig` (ou um arquivo assinado), abra Kleopatra.
2. Arraste o arquivo para a janela do Kleopatra **ou** clique com o botão direito no arquivo e escolha **Decrypt/Verify** / **Verify**.
3. Kleopatra mostrará se a assinatura é válida e qual chave a assinou (verifique o fingerprint e se a chave importada corresponde à pessoa).


# Boas práticas e dicas rápidas

* **Passphrase forte** na sua chave privada. Nunca compartilhe a chave privada.
* **Revocation certificate**: gere e guarde (Kleopatra oferece opção) — essencial se perder o acesso.
* **Backup**: exporte sua chave privada para backup seguro (arquivo criptografado, pendrive offline).
* **Confirme fingerprints** por um canal alternativo (chamada, pessoalmente) antes de confiar em uma chave.
* Use **RSA 4096** ou ECC (se preferir; ECC/Ed25519 é moderno) — RSA 4096 é compatível e seguro.
* Para integração com e-mail, use S/MIME ou OpenPGP dependendo do cliente; Gpg4win oferece plugins para Outlook.