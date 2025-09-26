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