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
🔹 1. General Splitter
![Fluxo](imagens/Screenshot_11.png)


<br>

🔹 2. Renomeando General Splitter
![Fluxo](imagens/Screenshot_12.png)
Nome: ```SPLIT_Course_List```


🔹 3. Configuração:
![Fluxo](imagens/Screenshot_13.png)
```
/courses/list/course
```

👉 Cada <course> será processado individualmente.


<br>

🔹 4. Removendo o Receiver
![Fluxo](imagens/Screenshot_14.png)

<br>

# 5️⃣Router
🔹 1. Router (XPath + Regex)
![Fluxo](imagens/Screenshot_15.png)

🔹 2. Renomeando Router 
![Fluxo](imagens/Screenshot_16.png)
```
Nome: ROUTER_Course_Type
```

🔹 3. Rotas configuradas:
![Fluxo](imagens/Screenshot_17.png)
➡️Rota Default
```
EXCEPTION_Handler
```

🔹 4. Marcardo como principal
![Fluxo](imagens/Screenshot_18.png)

<br>

# 6️⃣ Content Modifier

🔹 1. Renomeando o Content Modifie
![Fluxo](imagens/Screenshot_19.png)
```
CM_EXCEPTION_Handler
```

🔹 2. Adicionando HEADER 
![Fluxo](imagens/Screenshot_20.png)
```
Message Header
Create	- Content-Type	- Constant - application/xml
```

🔹 3. Adicionando Body:
![Fluxo](imagens/Screenshot_21.png)
```
<results_erro>
	${body}
</results_erro>
```

<br>

# 7️⃣Gather

🔹 1. Adicionar o Gather
![Fluxo](imagens/Screenshot_22.png)

🔹 2. Renomeando o Gather 
![Fluxo](imagens/Screenshot_23.png)
```
GATHER_Numeric
```
🔹 3. Configurando o Gather 
![Fluxo](imagens/Screenshot_24.png)
```
Aggregation Strategy
Gather Incoming Format: (XML Same Format)
Aggregation Algorithn: Combine 
```

🔹 4. Adicionando o Gather 
![Fluxo](imagens/Screenshot_25.png)
```
GATHER_Text
```
🔹 5. Adicionando o Gather 
![Fluxo](imagens/Screenshot_26.png)
```
GATHER_Business
```

🔹 7. Configuração do Gather
![Fluxo](imagens/Screenshot_27.png)

```
Aggregation Strategy
Gather Incoming Format: (XML Same Format)
Aggregation Algorithn: Combine 
```
<br>

# 8️⃣ Conectando ROUTER para GATHER

🔹 1. Conectando o Gather
![Fluxo](imagens/Screenshot_28.png)
```
Route Numerico
```

🔹 2. Configuração da conexão do Router para o Gather 
![Fluxo](imagens/Screenshot_29.png)
```
//course[matches(fee,'\d+')]
```

🔹 3. Conectando o Gather
![Fluxo](imagens/Screenshot_30.png)
```
Route Texto
```

🔹 4. Configuração da conexão do Router para o Gather 
![Fluxo](imagens/Screenshot_31.png)
```
//course[matches(fee,'[a-zA-Z]')]
```

🔹 5. Conectando o Gather
![Fluxo](imagens/Screenshot_32.png)
```
Route Business
```

🔹 6. Configuração da conexão do Router para o Gather 
![Fluxo](imagens/Screenshot_33.png)
```
//course[matches(name,'Business.+')]
```
<br>

# 9️⃣ Content Modifier
🔹 1. Adicionando o Content Modifier
![Fluxo](imagens/Screenshot_34.png)

🔹 2. Renomeando o  Content Modifier
![Fluxo](imagens/Screenshot_35.png)
```
CM_Format_Numeric
```

🔹 3. Adicionando o Header
![Fluxo](imagens/Screenshot_36.png)
```
Message Header
Create	- Content-Type	- Constant - application/xml
```
🔹 4. Adicionando Body:
![Fluxo](imagens/Screenshot_37.png)
```
<results>
	${body}
</results>
```

🔹 5. Copiar e Colar o Content Modifier
![Fluxo](imagens/Screenshot_38.png)

🔹 6. Renomeando a cópia
![Fluxo](imagens/Screenshot_39.png)
```
CM_Format_Text
```

🔹 7. Renomeando a cópia
![Fluxo](imagens/Screenshot_40.png)
```
CM_Format_Business
```

🔹 8. Ficando dessa forma a configurações da cópia
![Fluxo](imagens/Screenshot_41.png)


🔹 9. Conectando o GATHER no Content Modifier
![Fluxo](imagens/Screenshot_42.png)

🔹 10. Conectando o Content Modifier END
![Fluxo](imagens/Screenshot_43.png)

<br>

1️⃣0️⃣ Postman
🔹 1. Resultado do Postman
![Fluxo](imagens/Screenshot_44.png)

```
<results>
    <?xml version='1.0' encoding='UTF-8'?>
    <multimap:Messages xmlns:multimap="http://sap.com/xi/XI/SplitAndMerge">
        <multimap:Message1>
            <courses>
                <list>
                    <course>
                        <name>Programming</name>
                        <fee>300</fee>
                    </course>
                </list>
            </courses>
            <courses>
                <list>
                    <course>
                        <name>Business</name>
                        <fee>1200</fee>
                    </course>
                </list>
            </courses>
            <courses>
                <list>
                    <course>
                        <name>Business Advanced</name>
                        <fee>free</fee>
                    </course>
                </list>
            </courses>
        </multimap:Message1>
    </multimap:Messages>
</results>
```

<br>
<br>

---

## 📦 Exemplo prático – iFlow para baixar

📦 [Download do iFlow – Splitter-Router-com-XPath-e-Regex](https://github.com/souzajean/Splitter-Router-com-XPath-e-Regex/raw/Package/ZIF_COURSE_SPLIT_ROUTING.zip)


> O arquivo pode ser importado diretamente no SAP Integration Suite (CPI).






