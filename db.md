## Entidades de Configuração e Usuários

```
TipoUsuario:
- id (PK)
- nome+ - str
- descricao - text
- permissoes+ - json/list
- ativo+ - boolean

AnoEscolar:
- id (PK)
- ano+ - str

Estado:
- id (PK)
- nome+ - str
- sigla+ - str
- regiao+ - str

Cidade:
- id (PK)
- nome+ - str
- *estado_id (FK)

Instituicao:
- id (PK)
- nome+ - str
- tipo+ - str
- inep+ - str
- *cidade_id (FK)
- endereco - str
- telefone - str
- email - str

Usuario:
- id (PK)
- nome+ - str
- email+ - email (unique)
- cpf+ - str (unique, com validação)
- telefone+ - str
- data_nascimento+ - date
- *ano_escolar_id (FK, nullable para professores/coordenadores)
- *tipo_usuario_id (FK)
- *instituicao_id (FK, nullable)
- *cidade_id (FK)
- foto_perfil - str (URL/path)
- bio - text
- data_cadastro+ - datetime
- ultimo_acesso - datetime
- ativo+ - boolean
```

## Olimpíadas e Competições

```
AreaConhecimento:
- id (PK)
- nome+ - str
- cor_tema - str (hex color)
- icone - str

Olimpiada:
- id (PK)
- nome+ - str
- sigla+ - str
- descricao+ - text
- *area_conhecimento_id (FK)
- site_oficial+ - url
- logo - str (URL/path)
- nivel_nacional+ - boolean
- estados_permitidos+ - json/list
- anos_escolares_permitidos+ - json/list
- taxa_inscricao - decimal
- data_criacao+ - datetime
- *coordenador_responsavel_id (FK para Usuario)

RegulamentoOlimpiada:
- id (PK)
- *olimpiada_id (FK)
- versao+ - str
- arquivo_url+ - str
- data_publicacao+ - datetime

EdicaoOlimpiada:
- id (PK)
- *olimpiada_id (FK)
- ano+ - int
- data_inicio_inscricoes+ - datetime
- data_fim_inscricoes+ - datetime
- max_participantes - int
- status+ - str (inscrições_abertas, em_andamento, finalizada)
```

## Eventos e Calendário

```
Evento:
- id (PK)
- titulo+ - str
- descricao+ - text
- data_inicio+ - datetime
- data_fim - datetime
- tipo+ - str (inscricao, prova, resultado, aula, palestra)
- *olimpiada_id (FK, nullable)
- *edicao_olimpiada_id (FK, nullable)
- local - str
- virtual+ - boolean
- link_acesso - url
- *criador_id (FK para Usuario)
- publico+ - boolean
- notificar_participantes+ - boolean
```

## Inscrições e Resultados

```
Inscricao:
- id (PK)
- *usuario_id (FK)
- *edicao_olimpiada_id (FK)
- data_inscricao+ - datetime
- status+ - str (pendente, confirmada, cancelada)
- comprovante_pagamento - str (se houver taxa)
- observacoes - text

ResultadoParticipante:
- id (PK)
- *inscricao_id (FK)
- *edicao_olimpiada_id (FK)
- pontuacao - decimal
- classificacao - int
- medalha - str (ouro, prata, bronze, mencao)
- certificado_url - str
- data_resultado+ - datetime
```

## Recursos Educacionais

```
MaterialEstudo:
- id (PK)
- titulo+ - str
- descricao+ - text
- tipo+ - str (aula, simulado, exercicio, apostila, video)
- *area_conhecimento_id (FK)
- *autor_id (FK para Usuario - Professor)
- arquivo_url - str
- link_externo - url
- nivel_dificuldade+ - str (basico, intermediario, avancado)
- anos_escolares_alvo+ - json/list
- tags - json/list
- data_publicacao+ - datetime
- visualizacoes+ - int
- aprovado+ - boolean
- gratuito+ - boolean

AvaliacaoMaterial:
- id (PK)
- *material_id (FK)
- *usuario_id (FK)
- nota+ - int (1-5)
- comentario - text
- data_avaliacao+ - datetime
```

## Comunicação e Interação

```
GrupoDiscussao:
- id (PK)
- nome+ - str
- descricao+ - text
- *area_conhecimento_id (FK, nullable)
- *olimpiada_id (FK, nullable)
- *criador_id (FK para Usuario)
- publico+ - boolean
- max_membros - int
- data_criacao+ - datetime
- ativo+ - boolean

MembroGrupo:
- id (PK)
- *grupo_id (FK)
- *usuario_id (FK)
- papel+ - str (membro, moderador, admin)
- data_entrada+ - datetime
- ativo+ - boolean

MensagemGrupo:
- id (PK)
- *grupo_id (FK)
- *autor_id (FK para Usuario)
- conteudo+ - text
- arquivo_anexo - str
- data_envio+ - datetime
- editada+ - boolean
- data_edicao - datetime
- respondendo_mensagem_id (FK, self-reference)

Notificacao:
- id (PK)
- *usuario_destinatario_id (FK)
- titulo+ - str
- conteudo+ - text
- tipo+ - str (inscricao, evento, resultado, mensagem)
- *referencia_id (FK genérico)
- tipo_referencia+ - str (olimpiada, evento, grupo, etc.)
- lida+ - boolean
- data_criacao+ - datetime
- data_leitura - datetime
```