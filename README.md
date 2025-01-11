# dados_powerbi

# Guia de Transformação de Dados no Power BI

## 1. Configuração Inicial
### Azure MySQL
1. Criar uma instância de Azure MySQL
   - Escolher tier básico para teste
   - Configurar firewall para permitir conexão do Power BI
   - Anotar as credenciais de conexão

### Importação do Banco de Dados
```sql
-- Verificar gerentes e departamentos
SELECT 
    e.Fname, 
    e.Lname, 
    e.Super_ssn,
    d.Mgr_ssn
FROM 
    employee e
LEFT JOIN 
    department d ON e.Ssn = d.Mgr_ssn;
```

## 2. Transformações no Power BI

### Verificações Iniciais
1. Tipos de dados:
   - Converter campos monetários para decimal fixo
   - Verificar formatos de data
   - Ajustar tipos de texto e números

### Tratamento de Nulos
1. Análise de Super_ssn nulo:
   - Identificar funcionários sem gerente
   - Confirmar se são realmente gerentes
2. Departamentos sem gerente:
   - Preencher dados faltantes
   - Documentar alterações

### Mesclagens e Transformações
1. Employee + Department:
```
- Mesclar usando Dnumber = Dno
- Manter apenas colunas relevantes
- Tipo de junção: Left Outer
```

2. Nome completo do funcionário:
```
- Mesclar Fname + Lname
- Usar separador de espaço
- Nome da nova coluna: FullName
```

3. Departamento + Localização:
```
- Mesclar Dname + Dlocation
- Criar identificador único
- Usar separador apropriado
```

### Agrupamentos
1. Contagem de funcionários por gerente:
```
- Agrupar por Mgr_ssn
- Calcular contagem de funcionários
- Criar hierarquia se necessário
```

### Limpeza Final
1. Remover colunas desnecessárias:
   - Manter apenas campos relevantes para relatório
   - Documentar colunas removidas
   - Verificar dependências

## 3. Observações Importantes

1. Uso de Mesclar vs. Atribuir:
   - Mesclar é usado quando precisamos combinar colunas mantendo relacionamentos
   - Atribuir seria inadequado pois geraria duplicação desnecessária
   - Para departamento-localização, mesclar preserva a estrutura original

2. Boas Práticas:
   - Documentar todas as transformações
   - Manter consistência nos nomes
   - Verificar performance das consultas
   - Criar medidas quando necessário
