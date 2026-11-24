# Extrato financeiro — configuração Firebase
O frontend usa Firestore (financial_statements) e Cloud Storage (financial_statements/{id}/{arquivo}). Marcelo é administrador; Cauã e Edson são leitores. Não publique documentos reais antes de verificar as regras no console Firebase. O GitHub não aplica essas regras automaticamente.

## Firestore
Adicionar dentro de match /databases/{database}/documents, preservando as regras existentes de leads. Remover ou restringir quaisquer regras genéricas que também concedam acesso a esta coleção (regras allow são cumulativas).
```
match /financial_statements/{id} {
  allow read: if request.auth != null && request.auth.token.email in ['marcelo@ativacao.com','caua@ativacao.com','edson@ativacao.com'];
  allow create, update, delete: if request.auth != null && request.auth.token.email == 'marcelo@ativacao.com';
}
```
## Storage
Ativar o bucket existente, se necessário. Adicionar dentro de match /b/{bucket}/o, preservando regras de outros caminhos e restringindo regras genéricas sobrepostas.
```
match /financial_statements/{id}/{file} {
  allow read: if request.auth != null && request.auth.token.email in ['marcelo@ativacao.com','caua@ativacao.com','edson@ativacao.com'];
  allow create, update: if request.auth != null && request.auth.token.email == 'marcelo@ativacao.com' && request.resource.size <= 10 * 1024 * 1024 && request.resource.contentType in ['application/pdf','application/vnd.ms-excel','application/vnd.openxmlformats-officedocument.spreadsheetml.sheet'];
  allow delete: if request.auth != null && request.auth.token.email == 'marcelo@ativacao.com';
}
```
Configurar CORS do bucket para permitir GET da origem real do site (getBlob utiliza requisição autenticada). Verificar plano de faturamento exigido pelo Storage. Testar upload, download, edição, exclusão e acesso negado a leitores para operações de escrita. Não foi realizado deploy de regras nem teste autenticado em produção.
