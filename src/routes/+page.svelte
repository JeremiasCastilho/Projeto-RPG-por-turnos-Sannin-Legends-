
<script lang="ts">
    // Importa os estilos CSS responsáveis pela aparência do menu principal.
    import "../styles/menu.css"

    // Armazena o título principal do jogo.
    let tituloDoJogo: string = "SANNIN LEGENDS"

    // Armazena o subtítulo exibido abaixo do título.
    let subtitulo: string = "THE LEGENDARY THREE"
</script>

<!-- Área principal que recebe a imagem de fundo do menu. -->
<div class="fundo-menu">

    <!-- Agrupa e centraliza o emblema, os títulos e os botões. -->
    <div class="conteudo-menu">

        <!-- Exibe o emblema dos três Sannin. -->
        <img
            class="emblema-sannin"
            src="/images/sannin.png"
            alt="Sannin emblem"
        />

        <!-- Exibe o título armazenado na variável tituloDoJogo. -->
        <h1>{tituloDoJogo}</h1>

        <!-- Exibe o subtítulo armazenado na variável subtitulo. -->
        <h2>{subtitulo}</h2>

        <!-- Agrupa os links de navegação do menu principal. -->
        <nav class="botoes-menu">

            <!-- Leva o jogador para a página de batalha. -->
            <a class="botao-menu" href="/jogar">PLAY</a>

            <!-- Leva o jogador para a página com informações do projeto. -->
            <a class="botao-menu" href="/sobre">ABOUT</a>

        </nav>

    </div>

</div>
