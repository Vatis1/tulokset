<!DOCTYPE html>
<html>
<head>
    <title>Kuntosalitreenin Tuloslaskuri</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</head>
<body>
    <div class="container mt-5">
        <h1 class="text-center">Syötä treenitulos ja toistomäärä</h1>
        <form class="mb-4">
            <div class="form-group">
                <label for="harjoitus">Harjoitus:</label>
                <select id="harjoitus" class="form-control">
                    <option value="maastaveto">Maastaveto</option>
                    <option value="leuanveto">Leuanveto</option>
                    <option value="penkkipunnerrus">Penkkipunnerrus</option>
                    <option value="pystypunnerrus">Pystypunnerrus</option>
                </select>
            </div>
            <div class="form-group">
                <label for="tulos">Tulos (kg):</label>
                <input type="number" id="tulos" class="form-control" min="0">
            </div>
            <div class="form-group">
                <label for="toistot">Toistomäärä:</label>
                <input type="number" id="toistot" class="form-control" min="0">
            </div>
            <button type="button" class="btn btn-primary" onclick="tallennaTulos()">Tallenna</button>
        </form>

        <h2 class="text-center">Tallennetut Tulokset</h2>
        <ul id="tallennetutTulokset" class="list-group">
            <!-- Tähän näytetään tallennetut tulokset -->
        </ul>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        // Tallennettujen tulosten kaaviot
        const tulosKaaviot = {
            maastaveto: null,
            leuanveto: null,
            penkkipunnerrus: null,
            pystypunnerrus: null,
        };

        function alustaTulosKaavio(harjoitus) {
            const ctx = document.getElementById(`${harjoitus}-kaavio`).getContext('2d');
            tulosKaaviot[harjoitus] = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: [],
                    datasets: [{
                        label: 'Tulos (kg)',
                        data: [],
                        backgroundColor: 'rgba(75, 192, 192, 0.2)',
                        borderColor: 'rgba(75, 192, 192, 1)',
                        borderWidth: 1,
                    }],
                },
                options: {
                    scales: {
                        y: {
                            beginAtZero: true,
                        },
                    },
                },
            });
        }

        function lisaaTulosKaavioon(harjoitus, tulos, toistot) {
            if (tulosKaaviot[harjoitus] === null) {
                alustaTulosKaavio(harjoitus);
            }

            tulosKaaviot[harjoitus].data.labels.push(`${tulos} kg (${toistot} toistoa)`);
            tulosKaaviot[harjoitus].data.datasets[0].data.push(tulos);
            tulosKaaviot[harjoitus].update();
        }

        function tallennaTulos() {
            const harjoitus = document.getElementById('harjoitus').value;
            const tulos = document.getElementById('tulos').value;
            const toistot = document.getElementById('toistot').value;

            if (harjoitus !== '' && tulos !== '' && toistot !== '') {
                const tulosTeksti = `${harjoitus}: ${tulos} kg (${toistot} toistoa)`;
                const tuloksetLista = document.getElementById('tallennetutTulokset');
                const uusiTulos = document.createElement('li');
                uusiTulos.textContent = tulosTeksti;
                uusiTulos.classList.add('list-group-item');
                tuloksetLista.appendChild(uusiTulos);

                // Lisätään tulos kaavioon
                lisaaTulosKaavioon(harjoitus, tulos, toistot);

                // Tallennetaan tiedot selaimen paikalliseen tallennustilaan
                tallennaSelaimenTietoihin(tulosTeksti);

                // Tyhjennetään syötekentät
                document.getElementById('harjoitus').value = '';
                document.getElementById('tulos').value = '';
                document.getElementById('toistot').value = '';
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
                const tallennetutTulokset = JSON.parse(localStorage.tulokset);
                const tuloksetLista = document.getElementById('tallennetutTulokset');
                tallennetutTulokset.forEach(function (tulos) {
                    const uusiTulos = document.createElement('li');
                    uusiTulos.textContent = tulos;
                    uusiTulos.classList.add('list-group-item');
                    tuloksetLista.appendChild(uusiTulos);

                    // Lisätään tulos kaavioon tallennetun tiedon perusteella
                    const harjoitus = tulos.split
