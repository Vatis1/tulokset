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
                <input type="number" id="toistot" class "form-control" min="0">
            </div>
            <button type="button" class="btn btn-primary" onclick="tallennaTulos()">Tallenna</button>
        </form>

        <h2 class="text-center">Tallennetut Tulokset</h2>
        <ul id="tallennetutTulokset" class="list-group">
            <!-- Tähän näytetään tallennetut tulokset -->
        </ul>
    </div>

    <script>
        // ... (muu koodi säilyy ennallaan)
    </script>
</body>
</html>
