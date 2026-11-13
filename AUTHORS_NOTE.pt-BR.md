# Notas do Autor — Projeto Estação de IA Local

> **Autor:** Thiago Wingerter  
> **Versão de Referência:** `v0.1.0` (Baseline Local Ativa)

---

Olá a todos,

Meu nome é Thiago Wingerter e estou desenvolvendo com ajuda de AI's de fronteira minha própria AI local. A ideia é simples: não pagar por tokens ou assinaturas (mas ainda pagar a conta de luz!), não ter o conteúdo anexado ou absorvido pelas empresas detentoras (um pouco de privacidade), não ser restringido por regras e safeguards corporativos (inovação precisa de um pouco de espaço e rebeldia) e, por último mas não menos importante: aprender a estrutura por trás dos modelos e ter a capacidade de criar, customizar e evoluir estruturas ao longo do tempo.

Como podem ver, isso não é algo teórico: o que descrevo na versão `1.0` já está ativo e funcional na minha máquina local. A config que será utilizada como referência (**Tier 3** na tabela de hardware) é a minha máquina doméstica, devidamente configurada e que já está produzindo localmente.

Mas afinal, o que tenho em mente com uma IA local? A resposta é simples: além da curiosidade e vontade de aprender mais na prática, eu sempre gostei muito de jogos... e vi com a AI a possibilidade de desenvolver o meu próprio game. Para isso já tenho um ambiente isolado no **Stability Matrix** rodando um **ComfyUI** com um checkpoint derivado do PonyV6 (`autismmixSDXL_autismmixConfetti`). Isso me permite criar toda a parte de arte e assets (imagens), já que a natureza não me rendeu este dom na minha ficha de personagem neste mundo.

Antes que ocorram julgamentos, eu entendo e respeito a criação artística individual e humana e sei que o processo de criação de imagem de AI nada mais é que algoritmos que copiam trabalhos artísticos pré-avaliados. Eu não quero desmerecer ou invalidar o trabalho destes criadores e artistas, mas entenda que para um projeto individual que ainda está na fase de estudo e que não disponho de valores financeiros para arcar os custos de arte (e também não conheço ninguém que queira desenhar de graça no momento), o uso da AI permite que eu não fique estagnado e avance com minhas ideias e descobertas, abrindo um horizonte de novas opções além dos simples assets de um RPG Maker, por exemplo (que já usei muito, diga-se de passagem...) ou imagens recicladas para um projeto Ren'Py.

---

### Estado Atual e Próximos Passos

Bem, inicialmente é isto... devo manter este projeto atualizado ao longo do tempo, adicionar curiosidades, um mapa de entendimento dos termos para leigos ou iniciantes no mundo de AI, próximos passos e, claro... os resultados desta empreitada. 

**Em breve:**
* O núcleo já funciona localmente: já consigo conversar pelo **Continue** e gerar projetos, criar e editar arquivos e desenvolver códigos automatizados pelo **Cline** (ambos dentro do VS Code).
* Mesmo o núcleo estando operacional, estou sem braços no momento (nenhum MCP específico) e sem acesso à internet (*air-gapped*).
* O **ComfyUI** está isolado e configurado manualmente por mim e também só gera imagens manualmente por enquanto (Ah! Já temos um LoRA configurado para as imagens — treinei no Civitai o primeiro modelo).

Continue acompanhando o projeto e vamos discutir ao longo do tempo. Estarei trabalhando e aberto a ideias.

**CYA**