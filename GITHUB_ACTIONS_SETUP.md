# GitHub Actions Build Setup para Madeira

Este documento explica como usar o workflow do GitHub Actions para buildar o Madeira e gerar o arquivo `.ipa`.

## 📋 Pré-requisitos

- ✅ Fork do repositório Madeira
- ✅ Apple ID (gratuito)
- ✅ Acesso ao GitHub Actions (incluído em repositórios públicos)

## 🔑 Configuração de Secrets

O workflow precisa de credenciais Apple para assinar o app. Adicione os seguintes secrets no seu repositório:

### Como adicionar Secrets:
1. Vá para **Settings** → **Secrets and variables** → **Actions**
2. Clique em **New repository secret**
3. Adicione cada secret abaixo:

| Nome do Secret | Valor | Onde encontrar |
|---|---|---|
| `APPLE_ID` | Seu email Apple | appleid.apple.com |
| `APPLE_PASSWORD` | App-specific password | appleid.apple.com → Security → App-specific passwords |
| `APPLE_TEAM_ID` | Team ID | developer.apple.com (seu perfil) |

### Gerar App-specific password (IMPORTANTE):
```
1. Acesse https://appleid.apple.com/account/manage
2. Sign In com sua Apple ID
3. Vá para Security → App-specific passwords
4. Gere uma senha para "GitHub Actions" ou "macOS"
5. Copie a senha gerada (aparece apenas uma vez)
6. Use esta senha no secret APPLE_PASSWORD, NÃO sua senha real
```

## 🚀 Como usar o Workflow

### Opção 1: Build automático a cada push
O workflow roda automaticamente quando você faz push para `main` ou `github-actions-build`.

### Opção 2: Build manual (Workflow Dispatch)
1. Vá para **Actions** no seu repositório
2. Selecione **Build Madeira IPA**
3. Clique em **Run workflow**
4. Escolha a branch
5. Clique em **Run workflow**

### Opção 3: Build ao criar Release
Para buildar automaticamente quando criar uma release:
```bash
git tag v1.0.0
git push origin v1.0.0
```

## 📥 Baixar o IPA

Após o workflow ser concluído:

1. Vá para **Actions** → selecione a run
2. Role até **Artifacts**
3. Baixe `Madeira.ipa`
4. Também está disponível `Madeira.xcarchive` (backup)

## ⚙️ O que o Workflow faz

```
1. Clona o repositório com todos os submodules
2. Configura o Xcode
3. Instala dependências (cmake, ninja, pkg-config, etc)
4. Build das bibliotecas Wine
5. Build do FEX-Emu
6. Build do DXMT
7. Compila o app iOS
8. Gera o arquivo .ipa
9. Faz upload como artefato
10. Cria Release no GitHub (se for tag)
```

## ⏱️ Tempo de Build

- **Primeira vez**: 3-4 horas (dependências são compiladas)
- **Builds subsequentes**: 1-2 horas (cache de dependências)

## 🔴 Troubleshooting

### ❌ Erro: "Xcode project not found"
- Verifique se o arquivo `.xcodeproj` ou `.xcworkspace` existe em `/app`
- Atualize o `scheme` no workflow se necessário

### ❌ Erro: "Apple ID or password is incorrect"
- Verifique se adicionou os secrets corretamente
- Garanta que criou uma **app-specific password**, não a senha da conta
- App-specific passwords não funcionam com 2FA desativado

### ❌ Erro: "Provisioning profile expired"
- Se usar Apple ID gratuito, o profile expira em 7 dias
- Faça rebuild do workflow dentro de 7 dias
- Ou inscreva-se no Apple Developer Program ($99/ano)

### ❌ Erro de timeout no build
- Aumentar o timeout no workflow
- Ou executar builds localmente se possível

## 💡 Dicas

- Sempre use `--recurse-submodules` ao clonar o Madeira
- Mantenha os secrets privados (nunca compartilhe)
- Verifique os logs do workflow para erros específicos
- O arquivo IPA pode ser grande (500MB+)

## 📝 Notas Importantes

- ⚠️ Provisioning profiles com Apple ID gratuito expiram em 7 dias
- ⚠️ O IPA gerado é apenas para desenvolvimento (sideloading)
- ⚠️ Você precisa de um iPhone real para instalar (com debugger JIT)
- ⚠️ Nunca compartilhe seus secrets ou credenciais

## 🔗 Recursos Úteis

- [Apple Developer Account](https://developer.apple.com)
- [GitHub Actions Documentation](https://docs.github.com/actions)
- [Madeira Repository](https://github.com/willfaust/Madeira)
- [StikDebug (JIT debugger)](https://github.com/0-Blu/StikJIT)

## ❓ Próximos Passos

1. ✅ Adicione os secrets ao repositório
2. ✅ Vá para **Actions** e dispare o workflow manualmente
3. ✅ Acompanhe os logs para ver o progresso
4. ✅ Baixe o `.ipa` quando terminar
5. ✅ Use o StikDebug para instalar no iPhone

---

**Status**: Workflow pronto para uso! 🎉
