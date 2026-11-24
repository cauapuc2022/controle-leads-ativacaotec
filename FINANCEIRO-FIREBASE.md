# Extrato financeiro no Firebase

O extrato usa o Firestore para o histórico e os anexos no projeto `controle-leads-ativacaotec`. O upload de novos arquivos funciona no plano Spark, dentro das cotas do banco já utilizado pelos leads. Cada PDF, XLS ou XLSX de até 10 MB é dividido em documentos binários de até 768 KiB. O histórico carrega os metadados, e os bytes são lidos apenas ao baixar um arquivo.

Cauã, Marcelo e Edson podem visualizar, publicar, editar, baixar e excluir lançamentos. A autenticação e a autorização são verificadas pelo Firebase. As permissões da coleção de leads seguem a regra já publicada no projeto.

## Publicação

Publique `firestore.rules` no banco padrão usando o Console do Firebase ou `firebase deploy --only firestore:rules --project controle-leads-ativacaotec`. Confirme antes se a regra de leads no console ainda corresponde à regra versionada. A publicação do HTML segue o fluxo de hospedagem do repositório. Novos anexos dispensam Cloud Storage e CORS.

Registros antigos com referências do Cloud Storage continuam usando o caminho antigo. O download e a limpeza desses arquivos dependem de o bucket original continuar acessível.

## Verificação

Com um arquivo de teste sem dados reais, valide upload, download, edição e exclusão nas contas autorizadas. Confira se as três contas veem o mesmo histórico e se uma conta fora da lista recebe acesso negado a lançamentos e anexos. Confira também a tela de leads. O armazenamento e o tráfego dos anexos compartilham a cota do Firestore com o restante do sistema.
