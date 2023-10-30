<!DOCTYPE html>
<html>
<head>
    <title>Kuntosalitreenin Tuloslaskuri</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
    <style>
        body {
            background-image: url('https://previews.123rf.com/images/wavebreakmediamicro/wavebreakmediamicro1509/wavebreakmediamicro150952301/45650930-two-fit-people-on-the-background-in-crossfit-gym.jpg');
            background-size: cover;
        }
        .container {
            background-color: rgba(255, 255, 255, 0.7);
            padding: 20px;
            border-radius: 10px;
        }
        .form-group label {
            color: black;
            font-weight: bold;
        }
        h1 {
            color: black;
            text-decoration: underline;
        }
        .btn-primary {
            background-color: #007bff;
        }
        .btn-primary:hover {
            background-color: #0056b3;
        }
        .list-group-item {
            color: black;
            font-weight: bold;
        }
        /* Lisää jokaiselle liikkeelle oma taustaväri */
        .penkkipunnerrus {
            background-color: #ffcccb; /* Punainen */
        }
        .pystypunnerrus {
            background-color: #c2f0c2; /* Vihreä */
        }
        .maastaveto {
            background-color: #c2c2f0; /* Sininen */
        }
        .leuanveto {
            background-color: #f0c2f0; /* Vaaleanpunainen */
        }
        .kyykky {
            background-color: #f0f0c2; /* Keltainen */
        }
    </style>
</head>
<body>
    <div class="container mt-5">
        <h1 class="text-center">Syötä treenitulos</h1>
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

            if (harjoitus !== '' && paino !== '' && toistot !== '') {
                const paivamaara = new Date();
                const formattedDate = `${paivamaara.getDate()}/${paivamaara.getMonth() + 1}/${paivamaara.getFullYear()}`;
                const tulosTeksti = `${harjoitus}: ${paino} kg, ${toistot} toistoa, ${formattedDate}`;
                const tuloksetLista = document.getElementById('tallennetutTulokset');
                const uusiTulos = document.createElement('li');
                uusiTulos.textContent = tulosTeksti;
                uusiTulos.classList.add('list-group-item');
                uusiTulos.classList.add(harjoitus.toLowerCase()); // Lisää liikkeen luokka
                tuloksetLista.appendChild(uusiTulos);

                // Tallennetaan tiedot selaimen paikalliseen tallennustilaan
                tallennaSelaimenTietoihin(tulosTeksti);

                // Tyhjennetään syötekentät
                document.getElementById('harjoitus').value = '';
                document.getElementById('paino').value = '';
                document.getElementById('toistot').value = '';
            } else {
                alert('Täytä kaikki kentät ennen tallennusta.');
            }
        }

        function tallennaSelaimenTietoihin(tulos) {
            if (localStorage.tallennetutTulokset) {
                const tallennetutTulokset = JSON.parse(localStorage.tallennetutTulokset);
                tallennetutTulokset.push(tulos);
                localStorage.tallennetutTulokset = JSON.stringify(tallennetutTulokset);
            } else {
                localStorage.tallennetutTulokset = JSON.stringify([tulos]);
            }
        }

        window.onload = function () {
            if (localStorage.tallennetutTulokset) {
                const tallennetutTulokset = JSON.parse(localStorage.tallennetutTulokset);
                const tuloksetLista = document.getElementById('tallennetutTulokset');
                tallennetutTulokset.forEach(function (tulos) {
                    const uusiTulos = document.createElement('li');
                    uusiTulos.textContent = tulos;
                    uusiTulos.classList.add('list-group-item');
                    // Etsi liikkeen nimi tekstistä ja lisää se luokkana
                    const liikkeenNimi = tulos.split(':')[0].trim().toLowerCase();
                    uusiTulos.classList.add(liikkeenNimi);
                    tuloksetLista.appendChild(uusiTulos);
                });
            }
        }
    </script>
</body>
</html>
