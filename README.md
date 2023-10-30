<!DOCTYPE html>
<html>
<head>
    <title>Kuntosalitreenin Tuloslaskuri</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</head>
<body>
    <div class="container mt-5">
        <h1 class="text-center">Syötä treenitulos ja viikko</h1>
        <form class="mb-4">
            <div class="form-group">
                <label for="harjoitus">Harjoitus:</label>
                <select id="harjoitus" class="form-control">
                    <option value="penkkipunnerrus">Penkkipunnerrus</option>
                    <option value="pystypunnerrus">Pystypunnerrus</option>
                    <option value="maastaveto">Maastaveto</option>
                    <option value="leuanveto">Leuanveto</option>
                    <option value="kyykky">Kyykky</option>
                </select>
            </div>
            <div class="form-group">
                <label for="paino">Paino (kg):</label>
                <input type="number" id="paino" class="form-control" min="0">
            </div>
            <div class="form-group">
                <label for="toistot">Toistomäärä:</label>
                <input type="number" id="toistot" class="form-control" min="0">
            </div>
            <div class="form-group">
                <label for="viikko">Viikko:</label>
                <input type="number" id="viikko" class="form-control" min="1">
            </div>
            <button type="button" class="btn btn-primary" onclick="tallennaTulos()">Tallenna</button>
        </form>

        <h2 class="text-center">Tallennetut Tulokset</h2>
        <ul id="tallennetutTulokset" class="list-group">
            <!-- Tähän näytetään tallennetut tulokset -->
        </ul>
    </div>

    <script>
        function tallennaTulos() {
            const harjoitus = document.getElementById('harjoitus').value;
            const paino = document.getElementById('paino').value;
            const toistot = document.getElementById('toistot').value;
            const viikko = document.getElementById('viikko').value;

            if (harjoitus !== '' && paino !== '' && toistot !== '' && viikko !== '') {
                const tulosTeksti = `${harjoitus}: ${paino} kg, ${toistot} toistoa, Viikko ${viikko}`;
                const tuloksetLista = document.getElementById('tallennetutTulokset');
                const uusiTulos = document.createElement('li');
                uusiTulos.textContent = tulosTeksti;
                uusiTulos.classList.add('list-group-item');
                tuloksetLista.appendChild(uusiTulos);

                // Tallennetaan tiedot selaimen paikalliseen tallennustilaan
                tallennaSelaimenTietoihin(tulosTeksti);

                // Tyhjennetään syötekentät
                document.getElementById('harjoitus').value = '';
                document.getElementById('paino').value = '';
                document.getElementById('toistot').value = '';
                document.getElementById('viikko').value = '';
            } else {
                alert('Täytä kaikki kentät ennen tallennusta.');
            }
        }

        function tallennaSelaimenTietoihin(tulos) {
            // Tallennetaan tulokset selaimen paikalliseen tallennustilaan
            if (localStorage.tulokset) {
                const tallennetutTulokset = JSON.parse(localStorage.tulokset);
                tallennetutTulokset.push(tulos);
                localStorage.tulokset = JSON.stringify(tallennetutTulokset);
            } else {
                localStorage.tulokset = JSON.stringify([tulos]);
            }
        }

        // Ladataan tallennetut tiedot sivun latautuessa
        window.onload = function () {
            if (localStorage.tulokset) {
                const tallennetutTulokset = JSON.parse
