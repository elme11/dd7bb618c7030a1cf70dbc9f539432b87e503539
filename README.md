<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>بحث جوجل مع اقتراحات</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        input[type="text"] { width: 300px; padding: 10px; font-size: 16px; }
        button { padding: 10px 15px; font-size: 16px; }
        .suggestions { border: 1px solid #ccc; max-width: 300px; position: absolute; z-index: 10; background: white; }
        .suggestion-item { padding: 10px; cursor: pointer; }
        .suggestion-item:hover { background-color: #f0f0f0; }
    </style>
</head>
<body>

<div class="container" style="position: relative;">
    <h1>بحث جوجل مع اقتراحات</h1>
    <input type="text" id="searchQuery" placeholder="اكتب ما تبحث عنه...">
    <button id="searchButton">بحث</button>
    <div id="suggestions" class="suggestions" style="display: none;"></div>
</div>

<script>
    const apiKey = 'YOUR_API_KEY'; // استبدل هذا بمفتاح API الخاص بك
    const cx = 'YOUR_CX'; // استبدل هذا بمعرف محرك البحث الخاص بك

    const searchQuery = document.getElementById('searchQuery');
    const suggestionsBox = document.getElementById('suggestions');

    searchQuery.addEventListener('input', function() {
        const query = this.value;
        if (query) {
            fetch(`https://www.googleapis.com/customsearch/v1/suggest?q=${query}&key=${apiKey}&cx=${cx}`)
                .then(response => response.json())
                .then(data => {
                    suggestionsBox.innerHTML = '';
                    if (data && data.suggestions) {
                        data.suggestions.forEach(suggestion => {
                            const div = document.createElement('div');
                            div.classList.add('suggestion-item');
                            div.textContent = suggestion;
                            div.onclick = function() {
                                searchQuery.value = suggestion;
                                suggestionsBox.style.display = 'none';
                            };
                            suggestionsBox.appendChild(div);
                        });
                        suggestionsBox.style.display = 'block';
                    } else {
                        suggestionsBox.style.display = 'none';
                    }
                })
                .catch(error => {
                    console.error('Error fetching suggestions:', error);
                });
        } else {
            suggestionsBox.style.display = 'none';
        }
    });

    document.getElementById('searchButton').addEventListener('click', function() {
        const query = searchQuery.value;
        if (query.trim()) {
            window.open(`https://www.google.com/search?q=${encodeURIComponent(query)}`, '_blank');
        } else {
            alert('الرجاء إدخال مصطلح البحث.');
        }
    });

    searchQuery.addEventListener('keypress', function(event) {
        if (event.key === 'Enter') {
            document.getElementById('searchButton').click();
        }
    });
</script>

</body>
</html>

