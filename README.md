# 🚀 SAP CPI – Splitter + Router com XPath e Regex
## SAP BTP CPI - Splitter + Router com XPath e Regex

Em cenários reais de integração no SAP BTP Integration Suite (Cloud Integration – CPI), é comum a necessidade de:

Processar listas de dados XML
Separar registros individualmente
Aplicar regras de roteamento dinâmico
Tratar diferentes tipos de conteúdo

Neste artigo, vamos construir um cenário completo utilizando:

✔️ Content Modifier <br>
✔️ General Splitter <br>
✔️ Router com XPath + Regex <br>
✔️ Tratamento por rotas <br> <br>
![Fluxo](imagens/capa-linkedin.png)
---


# :building_construction: Arquitetura do iFlow
<br>
:gear: Etapas da Integração
<br><br>

# 1️⃣Iflow

### Criando nosso Iflow
![Fluxo](imagens/Screenshot_1.png)
<br><br><br>

### Criando o Integration Flow
![Fluxo](imagens/Screenshot_2.png)
```
ZPKG_TRAINING_CPI_ROUTING
```
<br><br>

### Adicionando o Artefato do Integration Flow
![Fluxo](imagens/Screenshot_3.png)

<br><br>

### Criando o Integration Flow
![Fluxo](imagens/Screenshot_4.png)
```
ZIF_COURSE_SPLIT_ROUTING
```
<br>

### Editar o Iflow
![Fluxo](imagens/Screenshot_5.png)

### 🖥️ O fluxo foi desenvolvido no SAP Cloud Integration (CPI) seguindo as etapas abaixo.

<br>

# 2️⃣ HTTPS Sender
### Adicionando o HTTPS
![Fluxo](imagens/Screenshot_6.png)

<br>


### Configurando o HTTPS
O fluxo é iniciado através de um endpoint HTTPS, permitindo que aplicações externas consultem o serviço.
![Fluxo](imagens/Screenshot_7.png)
```
Address: /regex
```
<br>

# 3️⃣Content Modifier

🔹 1. Content Modifier – Payload inicial
![Fluxo](imagens/Screenshot_8.png)

<br>
🔹 2. Renomeando o Content Modifier

![Fluxo](imagens/Screenshot_9.png)

```
Nome: CM_Set_Initial_Payload
Tipo: Constant
```

🔹 3. Adicionando o conteúdo no Body
Responsável por simular a entrada de dados.
![Fluxo](imagens/Screenshot_10.png)
```
<courses>
    <list>
        <course>
            <name>Programming</name>
            <fee>300</fee>
        </course>
        <course>
            <name>Business</name>
            <fee>1200</fee>
        </course>
        <course>
            <name>Business Advanced</name>
            <fee>free</fee>
        </course>
    </list>
</courses>
```



<br>

# 4️⃣General Splitter
🔹 2. General Splitter
![Fluxo](imagens/Screenshot_11.png)

Nome: SPLIT_Course_List

Configuração:

/courses/list/course

👉 Cada <course> será processado individualmente.

🔹 3. Router (XPath + Regex)

Nome: ROUTER_Course_Type

Rotas configuradas:
🟢 Rota Numérica
matches(fee,'\d+')

👉 Identifica valores numéricos (ex: 300, 1200)

🔵 Rota Texto
matches(fee,'[a-zA-Z]')

👉 Identifica valores como "free"

🟣 Rota Business
matches(name,'Business.+')

👉 Identifica cursos com nome "Business"

🔹 4. Content Modifier por Rota

Cada rota encapsula o resultado:

📌 Numérico
<results>
    ${body}
</results>

📌 Texto
<results>
    ${body}
</results>

📌 Business
<results>
    ${body}
</results>


🔄 (Opcional) Gather por rota

Se necessário, você pode adicionar um Gather dentro de cada rota para consolidar os resultados.

📤 Resultado Final

Cada rota gera sua saída específica, por exemplo:

Numérico
<results>
    <course>
        <name>Programming</name>
        <fee>300</fee>
    </course>
</results>
Texto
<results>
    <course>
        <name>Business Advanced</name>
        <fee>free</fee>
    </course>
</results>
🧠 Conclusão

Esse cenário demonstra conceitos essenciais no SAP CPI:

Split de mensagens XML
Uso de XPath com Regex
Roteamento dinâmico
Boas práticas de modelagem
