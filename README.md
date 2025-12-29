## ⚒️ Languages and tools
### 💻 Languages
[![Programming Languages](https://skillicons.dev/icons?i=html,py)](https://skillicons.dev)
### 🔧 Tools and extras
[![Tools and extras](https://skillicons.dev/icons?i=azure,github,discord)](https://skillicons.dev)


<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Bubble Sort Infinito</title>
    <style>
        body { 
            font-family: sans-serif; 
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            justify-content: center;
            background: #121212; 
            height: 100vh;
            margin: 0;
        }
        #container { 
            display: flex; 
            align-items: flex-end; 
            height: 300px; 
            gap: 4px; 
            padding: 30px; 
            background: #1e1e1e; 
            border-radius: 12px; 
            box-shadow: 0 10px 30px rgba(0,0,0,0.5); 
        }
        .bar { 
            width: 15px; 
            background-color: #3498db; 
            border-radius: 2px 2px 0 0; 
            transition: background-color 0.1s; 
        }
        .active { background-color: #e74c3c !important; } /* Comparando */
        .sorted { background-color: #2ecc71 !important; } /* Ordenado */
    </style>
</head>
<body>

    <div id="container"></div>

    <script>
        const container = document.getElementById('container');
        // Criando uma lista de 30 valores aleatórios
        let array = Array.from({ length: 30 }, () => Math.floor(Math.random() * 100) + 5);
        let barras = [];

        function renderBars() {
            container.innerHTML = '';
            barras = [];
            array.forEach(valor => {
                const bar = document.createElement('div');
                bar.classList.add('bar');
                bar.style.height = `${valor * 2.5}px`;
                container.appendChild(bar);
                barras.push(bar);
            });
        }

        const sleep = (ms) => new Promise(resolve => setTimeout(resolve, ms));

        // Função para embaralhar o array (Reset)
        function shuffle(arr) {
            for (let i = arr.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [arr[i], arr[j]] = [arr[j], arr[i]];
            }
        }

        async function bubbleSortLoop() {
            while (true) { // Loop Infinito
                const n = array.length;
                
                for (let i = 0; i < n; i++) {
                    let swapped = false;
                    for (let j = 0; j < n - i - 1; j++) {
                        barras[j].classList.add('active');
                        barras[j+1].classList.add('active');
                        
                        await sleep(20); // Velocidade da animação

                        if (array[j] > array[j + 1]) {
                            [array[j], array[j + 1]] = [array[j + 1], array[j]];
                            
                            barras[j].style.height = `${array[j] * 2.5}px`;
                            barras[j+1].style.height = `${array[j+1] * 2.5}px`;
                            swapped = true;
                        }

                        barras[j].classList.remove('active');
                        barras[j+1].classList.remove('active');
                    }
                    
                    barras[n - i - 1].classList.add('sorted');
                    if (!swapped) {
                        // Pinta todas as que restam de verde
                        for(let k=0; k < n; k++) barras[k].classList.add('sorted');
                        break;
                    }
                }

                // Espera 2 segundos antes de recomeçar
                await sleep(2000);
                
                // Reinicia o processo
                shuffle(array);
                renderBars();
            }
        }

        renderBars();
        bubbleSortLoop(); // Inicia automaticamente
    </script>
</body>
</html>
