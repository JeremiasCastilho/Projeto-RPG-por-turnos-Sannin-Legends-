
<script lang="ts">
    // Importa os estilos CSS responsáveis pela aparência da página Sobre.
    import "../../styles/sobre.css"
</script>

<!-- Elemento principal que organiza toda a página Sobre. -->
<main class="pagina-sobre">

    <!-- Seção que reúne as informações sobre o jogo e sua equipe. -->
    <section class="conteudo-sobre">

        <!-- Cabeçalho com o emblema e o título da página. -->
        <div class="cabecalho-sobre">

            <!-- Linha decorativa acima do emblema. -->
            <div class="linha-decorativa"></div>

            <!-- Exibe a imagem do emblema dos três Sannin. -->
            <img
                class="emblema-sobre"
                src="/images/sannin.png"
                alt="Sannin emblem"
            />

            <!-- Exibe o subtítulo da página. -->
            <p class="subtitulo-sobre">
                THE LEGENDARY THREE
            </p>

            <!-- Exibe o título principal da seção. -->
            <h1>ABOUT THE GAME</h1>

            <!-- Linha decorativa abaixo do título. -->
            <div class="linha-decorativa"></div>

        </div>

        <!-- Apresenta uma descrição sobre a proposta e o funcionamento do jogo. -->
        <p class="descricao-sobre">
            Sannin Legends is a turn-based card RPG inspired by
            the legendary Sannin from the Naruto universe.

            Choose your character, use powerful abilities and
            summons, and challenge your opponent in battle.
        </p>

        <!-- Separa visualmente a descrição da seção da equipe. -->
        <div class="divisor-sobre"></div>

        <!-- Identifica a seção que apresenta os desenvolvedores. -->
        <h2>DEVELOPMENT TEAM</h2>

        <!-- Agrupa os cartões com os nomes dos integrantes do projeto. -->
        <div class="equipe-sobre">

            <!-- Cartão com as informações do primeiro integrante. -->
            <article class="integrante">
                <h3>Jeremias Castilho</h3>

                <!-- Link que permite abrir o programa de e-mail para contato. -->
                <a
                    class="email-integrante"
                    href="mailto:jbcj@igarassu.ifpe.edu.br"
                >
                    jbcj@igarassu.ifpe.edu.br
                </a>
            </article>

            <!-- Cartão com o nome do segundo integrante. -->
            <article class="integrante">
                <h3>Gabriel Ferreira</h3>
            </article>

            <!-- Cartão com o nome do terceiro integrante. -->
            <article class="integrante">
                <h3>Icaro Pereira</h3>
            </article>

            <!-- Cartão com o nome do quarto integrante. -->
            <article class="integrante">
                <h3>Daniel Lopes</h3>
            </article>

        </div>

        <!-- Link que retorna o usuário à página inicial do jogo. -->
        <a class="botao-voltar" href="/">BACK TO MENU</a>

    </section>

</main>
