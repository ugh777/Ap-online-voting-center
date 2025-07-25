<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" 
          content="width=device-width, initial-scale=1.0">
    <title>Online Voting</title>
    <link href=
"https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" 
          rel="stylesheet">
    <script src=
"https://cdn.jsdelivr.net/npm/chart.js@3.7.0/dist/chart.min.js">
      </script>
</head>

<body class="bg-gray-100 h-screen flex 
             flex-col justify-center items-center">
    <div class="bg-white p-8 rounded-lg shadow-md w-full 
                md:w-1/2 lg:w-1/3 border-2 border-green-600">
        <h1 class="text-3xl font-bold text-center mb-8">
              Online Voting
          </h1>
        <div class="flex flex-col mb-4">
            <label for="color" class="text-lg font-semibold mb-2">
                  Select Color:
              </label>
            <select id="color" 
                    class="border border-gray-300 rounded-md 
                           py-2 px-3 focus:outline-none">
                <option value="Red">Red House</option>
                <option value="Blue">Blue House</option>
                <option value="Green">Green House</option>
                <option value="Yellow">Yellow House</option>
            </select>
        </div>
        <button id="voteButton"
                class="bg-green-500 text-white px-6 py-2 
                       rounded-md self-center mt-4 focus:outline-none">
              Vote
          </button>
        <button id="clearButton"
            class="bg-red-500 text-white px-6 py-2 
                   rounded-md self-center mt-2 focus:outline-none">
              Clear Votes
          </button>
        <button id="pieChartButton"
            class="bg-purple-500 text-white px-6 py-2 
                   rounded-md self-center mt-2 focus:outline-none">
              Pie Chart
          </button>
        <div id="result" class="mt-8"></div>
        <div id="votes" class="mt-8">
            <h2 class="text-xl font-semibold mb-4">
                  Voted List:
              </h2>
        </div>
        <div class="w-64 h-64 mx-auto">
            <canvas id="pieChart"></canvas>
        </div>
    </div>
    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const colorDropdown = document.getElementById('color');
            const voteButton = document.getElementById('voteButton');
            const clearButton = document.getElementById('clearButton');
            const pieChartButton = document.getElementById('pieChartButton');
            const resultMessage = document.getElementById('result');
            const votedList = document.getElementById('votes');
            let myChart;
            voteButton.addEventListener('click', function () {
                const selectedColor = colorDropdown.value;
                let colorVotes = JSON.parse(localStorage
                                                .getItem('colorVotes')) || {};
                colorVotes[selectedColor] = (colorVotes[selectedColor] || 0)+1;
                localStorage.setItem('colorVotes', JSON.stringify(colorVotes));
                resultMessage.textContent = `You voted for 
                                             ${selectedColor} House.`;
                displayVotes();
            });
            clearButton.addEventListener('click', function () {
                localStorage.removeItem('colorVotes');
                resultMessage.textContent = 'All votes cleared.';
                displayVotes();
                if (myChart) {
                    myChart.destroy();
                }
            });
            pieChartButton.addEventListener('click', function () {
                const colorVotes = JSON.parse(localStorage
                                                    .getItem('colorVotes')) || {};
                const colors = Object.keys(colorVotes);
                const votes = Object.values(colorVotes);
                if (myChart) {
                    myChart.destroy();
                }
                const ctx = document.getElementById('pieChart')
                                    .getContext('2d');
                myChart = new Chart(ctx, {
                    type: 'pie',
                    data: {
                        labels: colors,
                        datasets: [{
                            label: 'Votes',
                            data: votes,
                            backgroundColor: [
                                'rgb(255, 99, 132)',
                                'rgb(54, 162, 235)',
                                'rgb(255, 205, 86)',
                                'rgb(75, 192, 192)',
                            ],
                            hoverOffset: 4
                        }]
                    },
                    options: {
                        plugins: {
                            title: {
                                display: true,
                                text: 'Voting Results'
                            }
                        }
                    }
                });
            });
            function displayVotes() {
                votedList.innerHTML = '';
                const colorVotes = JSON.parse(localStorage
                                              .getItem('colorVotes')) || {};
                for (const color in colorVotes) {
                    const voteItem = document.createElement('li');
                    voteItem.textContent = `${color} House: 
                                            ${colorVotes[color]}`;
                    votedList.appendChild(voteItem);
                }
            }
            displayVotes();
        });
    </script>
</body>

</html>
