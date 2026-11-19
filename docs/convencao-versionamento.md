# Convenção de Versionamento — CloudVault

## As 4 Regras do Repositório
1. **Repositório:** Hospedado no GitHub (`LuizFelipeDias/Projeto_Eng_Software`), com acesso de leitura/escrita para os 3 integrantes (`@LuizFelipeDias`, `@gustavomioduski11`, `@Samuelsdr`) e acesso garantido ao professor.
2. **Branches:** Nunca commitar diretamente na `main`. Criar uma branch por tarefa seguindo o padrão: `feat/<id-historia>-descricao-curta`, `fix/<descricao>` ou `docs/<descricao>`.
3. **Fusão (merge):** Toda alteração entra na `main` exclusivamente via Pull Request (PR) após revisão e aprovação de pelo menos **1 integrante humano** diferente do autor. O Arquiteto Técnico ou o Scrum Master realiza o merge.
4. **Teste e lint antes de integrar:** O GitHub Actions (CI) executa automaticamente `npm run lint` e `npm test` em todo PR. O merge só é permitido com ambos passando.

## Resolução de Conflitos
Se duas pessoas alterarem o mesmo trecho de código:
1. O autor do PR mais recente atualiza sua branch com a `main` (`git fetch origin && git rebase origin/main` ou `git merge origin/main`).
2. Resolve os conflitos localmente, conversando com o autor da outra alteração quando houver dúvida de regra de negócio.
3. Roda `npm test` e `npm run lint` novamente antes de dar push.

## Se o Build Quebrar na `main`
Seguindo a regra "nunca quebre a construção" (Sommerville, 2019, p. 678):
1. Interromper novos merges imediatamente.
2. Se a correção levar menos de 15 minutos, abrir PR `fix/build-main` prioritário; caso contrário, executar `git revert` no commit causador para restaurar a `main` verde.

## Responsabilidade pelo Código Integrado
> **Regra do grupo:** O integrante humano que aprova e integra o Pull Request na `main` responde integralmente pela qualidade, segurança e funcionamento do código integrado, mesmo quando gerado por agente de IA.
