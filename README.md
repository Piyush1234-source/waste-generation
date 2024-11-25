# waste-generation
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Waste Generation and Recycling Data</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            padding: 20px;
        }
        h1 {
            text-align: center;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        label, input {
            font-size: 18px;
        }
        input {
            padding: 10px;
            width: 100%;
            margin-top: 10px;
            margin-bottom: 20px;
            border-radius: 5px;
            border: 1px solid #ccc;
        }
        button {
            font-size: 18px;
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        .output {
            margin-top: 30px;
        }
        .output h3 {
            text-align: center;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Waste Generation and Recycling Data</h1>
    <label for="country">Enter the country name:</label>
    <input type="text" id="country" placeholder="e.g., Pakistan, Japan, Germany">
    <button onclick="getData()">Get Data</button>

    <div class="output" id="output"></div>
    <canvas id="wasteChart" width="400" height="200"></canvas>
</div>

<script>
    // Data for countries, waste generation, and recycling
    const data = {
        'Albania': { wasteGenerated: 5000000, wasteRecycled: 2000000 },
        'Andorra': { wasteGenerated: 1000000, wasteRecycled: 500000 },
        'Armenia': { wasteGenerated: 3000000, wasteRecycled: 1200000 },
        'Austria': { wasteGenerated: 8000000, wasteRecycled: 3500000 },
        'Azerbaijan': { wasteGenerated: 4000000, wasteRecycled: 1800000 },
        'Bahrain': { wasteGenerated: 1200000, wasteRecycled: 600000 },
        'Bangladesh': { wasteGenerated: 8000000, wasteRecycled: 2000000 },
        'Belgium': { wasteGenerated: 11000000, wasteRecycled: 3000000 },
        'Germany': { wasteGenerated: 80000000, wasteRecycled: 30000000 },
        'Japan': { wasteGenerated: 15000000, wasteRecycled: 8000000 },
        'Pakistan': { wasteGenerated: 2500000, wasteRecycled: 1000000 },
        // Add more countries as needed
    };

    // Global variable for chart to ensure it can be updated
    let wasteChart = null;

    // Function to get and display data
    function getData() {
        const country = document.getElementById('country').value.trim();
        const outputDiv = document.getElementById('output');
        const ctx = document.getElementById('wasteChart').getContext('2d');
        
        if (data[country]) {
            const { wasteGenerated, wasteRecycled } = data[country];
            const recyclingRate = ((wasteRecycled / wasteGenerated) * 100).toFixed(2);

            // Display data
            outputDiv.innerHTML = `
                <h3>Waste Data for ${country}</h3>
                <p>Waste Generated: ${wasteGenerated} tons</p>
                <p>Waste Recycled: ${wasteRecycled} tons</p>
                <p>Recycling Rate: ${recyclingRate}%</p>
            `;

            // Destroy the existing chart if it exists
            if (wasteChart) {
                wasteChart.destroy();
            }

            // Create new chart
            wasteChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['Waste Generated', 'Waste Recycled'],
                    datasets: [{
                        label: 'Waste Data (in tons)',
                        data: [wasteGenerated, wasteRecycled],
                        backgroundColor: ['rgba(255, 99, 132, 0.2)', 'rgba(75, 192, 192, 0.2)'],
                        borderColor: ['rgba(255, 99, 132, 1)', 'rgba(75, 192, 192, 1)'],
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    scales: {
                        y: {
                            beginAtZero: true,
                            ticks: {
                                callback: function(value) {
                                    return value.toLocaleString(); // Format numbers with commas
                                }
                            }
                        }
                    }
                }
            });
        } else {
            outputDiv.innerHTML = '<h3>Country not found. Please try again.</h3>';
            // Destroy chart if country not found
            if (wasteChart) {
                wasteChart.destroy();
            }
        }
    }
</script>

</body>
</html>
