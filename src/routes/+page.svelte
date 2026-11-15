<script lang="ts">
    import "../styles/menu.css"

    let tituloDoJogo: string = "SANNIN LEGENDS"
    let subtitulo: string = "THE LEGENDARY THREE"
</script>

<div class="fundo-menu">

    <div class="conteudo-menu">

        <img
            class="emblema-sannin"
            src="/images/sannin.png"
            alt="Sannin emblem"
        />

        <h1>{tituloDoJogo}</h1>

        <h2>{subtitulo}</h2>

        <nav class="botoes-menu">
            <a class="botao-menu" href="/jogar">PLAY</a>

            <a class="botao-menu" href="/sobre">ABOUT</a>
        </nav>

    </div>

</div>