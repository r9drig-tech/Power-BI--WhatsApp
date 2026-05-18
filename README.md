# 📲 Link do WhatsApp no Power BI (wa.me)

> Guia técnico completo para criar links clicáveis do WhatsApp diretamente em relatórios do Power BI usando DAX e categorização de URL.

---

## 📋 Visão Geral

O método `wa.me` permite que você adicione um link direto de conversa no WhatsApp em qualquer relatório Power BI — gratuitamente, sem APIs pagas. O usuário clica e abre uma conversa direta com o cliente.

**Compatibilidade:**
| Plataforma | Status |
|---|---|
| 🌐 Power BI Service (navegador) | ✅ Funciona |
| 📱 Power BI App Mobile | ✅ Funciona |
| 🖥️ Power BI Desktop | ❌ Não abre links externos |

---

## 🚀 Passo a Passo

### Passo 1 — Criar a Coluna Calculada DAX

A URL do WhatsApp segue o formato: `https://wa.me/5511999999999`

O número precisa estar no **formato internacional** sem espaços, parênteses ou traços.

```dax
URL WhatsApp =
VAR _numero =
    SUBSTITUTE(
        SUBSTITUTE(
            SUBSTITUTE(
                SUBSTITUTE(
                    SUBSTITUTE([ds_celular], " ", ""),
                ",", ""),
            "(", ""),
        ")", ""),
    "-", "")
VAR _numero_intl = "55" & _numero
RETURN
    "https://wa.me/" & _numero_intl
```

> ⚠️ Substitua `[ds_celular]` pelo nome da sua coluna que armazena o número de celular.

---

### Passo 2 — Categorizar a Coluna como URL da Web

Após criar a coluna calculada:

1. Acesse a **Visão de Dados** (ícone de tabela na barra lateral)
2. Selecione a coluna `URL WhatsApp`
3. Vá em **Ferramentas de Coluna** → **Categoria de Dados**
4. Escolha **URL da Web**

Isso ativa o comportamento de link clicável no relatório.

---

### Passo 3 — Usar no Relatório

#### Opção A — Tabela ou Matriz

1. Adicione a coluna `URL WhatsApp` ao visual de Tabela/Matriz
2. No painel de formatação do visual, ative **"Valor do URL"**
3. O ícone de link aparecerá em cada linha — clicável direto para o WhatsApp

#### Opção B — Botão

1. Insira um **Botão** no relatório
2. Em **Ação**, selecione **URL da Web**
3. Aponte para a medida ou coluna com a URL
4. Personalize o texto: _"Falar no WhatsApp"_

---

## ⚠️ Importante sobre Automação

| Método | Custo | Uso |
|---|---|---|
| `wa.me` (este guia) | **Gratuito** | Envio manual (1 a 1) |
| Z-API / 360dialog / Twilio | **Pago** | Disparos automáticos em massa |

O método `wa.me` **NÃO permite automação ou envio em massa**. Para isso, é necessário contratar uma API oficial do WhatsApp Business.

---

## 💡 Dicas Extras

- **Validação do número:** certifique-se que o campo de celular tem pelo menos 10 dígitos (DDD + número)
- **Já tem o +55?** Adicione um `IF` para evitar duplicar o código do país:

```dax
URL WhatsApp =
VAR _limpo =
    SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(
        SUBSTITUTE([ds_celular]," ",""),",",""),"(",""),")",""),"-","")
VAR _final =
    IF(LEFT(_limpo, 2) = "55", _limpo, "55" & _limpo)
RETURN "https://wa.me/" & _final
```

- **Teste sempre** no Power BI Service — links não funcionam no Desktop
- **Número com 9 dígito:** o Brasil exige o 9 na frente para celulares. Se o dado vier sem, adicione no DAX: `IF(LEN(_limpo) = 10, LEFT(_limpo,2) & "9" & RIGHT(_limpo,8), _limpo)`

---

## 📁 Estrutura do Repositório

```
📦 powerbi-whatsapp-link
 ┣ 📄 README.md           ← Este arquivo
 ┣ 📄 dax_url_whatsapp.txt       ← Fórmula DAX pronta para copiar
 ┗ 📄 exemplo_modelo.pbix        ← Modelo de exemplo (opcional)
```

---

## 🤝 Contribuições

Sinta-se à vontade para abrir uma **Issue** ou **Pull Request** com melhorias, casos de uso adicionais ou adaptações para outros países.

---

## 📄 Licença

MIT — use à vontade, só dê os créditos 😄

---

*Feito com 💚 para a comunidade Power BI Brasil*
