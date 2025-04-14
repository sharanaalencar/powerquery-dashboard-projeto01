# powerquery-dashboard-projeto01
Projeto de análise de dados com Power Query:  Tratamento e Visualização de Dados

# 🧼 Transformação e Visualização de Dados com Power Query

Este projeto simula um cenário real de tratamento de dados operacionais, com foco na automação e visualização de informações usando Power Query no Excel. Os dados ficticios foram estruturados para apoiar análises comerciais e operacionais.

---

## 📌 Funcionalidades

- Ajuste automático dos tipos de dados (texto, número e data)
- Limpeza de registros: remoção de vazios e duplicados
- Padronização de textos e capitalização
- Separação e formatação de CPFs válidos, mesmo em campos mistos com CNPJs
- Criação de dashboard com segmentações e gráficos dinâmicos
- Design com identidade visual personalizada
- Automação completa: sempre que a base for atualizada, o tratamento é feito automaticamente

---

## 🛠️ Ferramentas utilizadas

- Microsoft Excel
- Power Query (Editor de Consultas)
- Tabelas Dinâmicas
- Gráficos e Segmentações

---

## 🔍 Destaques do Power Query

Foi criado um script específico para identificar, extrair e formatar corretamente os CPFs em meio a textos misturados com CNPJs, preservando zeros à esquerda e aplicando a máscara padrão.
Código:

<let
    Texto = Text.Trim([cpf]),
    CPF_Inicio = Text.Start(Texto, 14),
    CPF_Fim = Text.Middle(Texto, Text.Length(Texto) - 14, 14),
    
    PadraoCPF = "[0-9]{3}\.[0-9]{3}\.[0-9]{3}-[0-9]{2}",
    PadraoCPF_SemPontuacao = "[0-9]{11}",
    PadraoCNPJ = "[0-9]{2}\.[0-9]{3}\.[0-9]{3}/[0-9]{4}-[0-9]{2}",
    
    TemApenasCNPJ = Text.Length(Texto) >= 18 and Text.Contains(Texto, "/") and not Text.Contains(Texto, "-"),
    
    CorrigirCPF = (cpf) => 
        let cpfCorrigido = Text.PadStart(cpf, 11, "0")
        in Text.Insert(Text.Insert(Text.Insert(cpfCorrigido, 3, "."), 7, "."), 11, "-"),
    
    CPF_Valido_Inicio = if Text.Length(CPF_Inicio) = 14 and Text.Contains(CPF_Inicio, ".") and Text.Contains(CPF_Inicio, "-") and not Text.Contains(CPF_Inicio, "/") then CPF_Inicio
                        else if Text.Length(CPF_Inicio) <= 11 and not TemApenasCNPJ then CorrigirCPF(CPF_Inicio)
                        else null,

    CPF_Valido_Fim = if Text.Length(CPF_Fim) = 14 and Text.Contains(CPF_Fim, ".") and Text.Contains(CPF_Fim, "-") and not Text.Contains(CPF_Fim, "/") then CPF_Fim
                     else if Text.Length(CPF_Fim) <= 11 and not TemApenasCNPJ then CorrigirCPF(CPF_Fim)
                     else null
in 
    if TemApenasCNPJ then null
    else if CPF_Valido_Inicio <> null then CPF_Valido_Inicio
    else if CPF_Valido_Fim <> null then CPF_Valido_Fim
    else null>

Esse script realiza as seguintes etapas:
Remove espaços desnecessários


Verifica se há um CPF no início ou fim do campo
Ignora CNPJs (identificados pelo “/”)
Aplica máscara padrão nos CPFs sem pontuação
> Em caso de inconsistência nos dados de entrada, o script retorna `null`, facilitando a identificação e o controle de qualidade.

---

## 📌 Como usar

1. Faça o download das planilhas (base e painel) 
2. Atualize a planilha de base de dados
3. Vá até "Atualizar Tudo" no Excel do painel
4. O painel será automaticamente atualizado e tratado

---

## 📎 Sobre

Este projeto foi criado para estudo e demonstração de boas práticas em análise de dados com ferramentas acessíveis.  
Ideal para quem trabalha com dados operacionais, supervisores, analistas e áreas comerciais.

---

## 🔗 Conecte-se

Me acompanhe também no [LinkedIn](https://linkedin.com/in/sharanaalencar) para mais projetos de dados e automações práticas.



