
Código do site (HTML + JavaScript)

<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Extrair Dados XML para Excel</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        .container {
            max-width: 600px;
            margin: auto;
        }
        .btn {
            margin-top: 10px;
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
        }
        .btn:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Extrair Dados de XML para Excel</h1>
        <input type="file" id="xmlFile" accept=".xml" />
        <button class="btn" onclick="processXML()">Gerar Excel</button>
        <p id="status"></p>
    </div>

    <script>
        function processXML() {
            const fileInput = document.getElementById('xmlFile');
            const status = document.getElementById('status');

            if (!fileInput.files.length) {
                status.textContent = "Por favor, selecione um arquivo XML.";
                return;
            }

            const file = fileInput.files[0];
            const reader = new FileReader();

            reader.onload = function (event) {
                const parser = new DOMParser();
                const xmlDoc = parser.parseFromString(event.target.result, "application/xml");

                // Exemplo: extraindo elementos do XML
                const rows = [];
                const elements = xmlDoc.getElementsByTagName("item"); // Mude "item" para o elemento do XML

                for (let i = 0; i < elements.length; i++) {
                    const name = elements[i].getElementsByTagName("name")[0]?.textContent || "N/A";
                    const value = elements[i].getElementsByTagName("value")[0]?.textContent || "N/A";
                    rows.push({ Nome: name, Valor: value });
                }

                if (rows.length > 0) {
                    generateExcel(rows);
                    status.textContent = "Planilha gerada com sucesso!";
                } else {
                    status.textContent = "Nenhum dado encontrado no arquivo XML.";
                }
            };

            reader.readAsText(file);
        }

        function generateExcel(data) {
            const worksheet = XLSX.utils.json_to_sheet(data);
            const workbook = XLSX.utils.book_new();
            XLSX.utils.book_append_sheet(workbook, worksheet, "Dados");

            XLSX.writeFile(workbook, "dados.xlsx");
        }
    </script>
</body>
</html>

Explicação do Código

1. Upload do arquivo XML: O <input type="file" /> permite que o usuário selecione um arquivo XML.


2. Leitura e análise do XML:

A API FileReader lê o conteúdo do arquivo.

A DOMParser analisa o XML e permite navegar pelos elementos.



3. Extração de dados:

É usado getElementsByTagName para buscar informações dentro de tags específicas no XML.

Os dados são armazenados em um array de objetos JSON.



4. Geração do Excel:

A biblioteca xlsx converte os dados em uma planilha Excel.

O método XLSX.writeFile permite baixar o arquivo gerado.




Como usar:

1. Copie o código acima para um arquivo index.html.


2. Abra o arquivo em um navegador.


3. Faça o upload de um arquivo XML no formato esperado (com tags como <item><name>...</name><value>...</value></item>).


4. Clique no botão para gerar o Excel.



