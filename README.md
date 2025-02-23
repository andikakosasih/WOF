<script>
    const sheetId = "1X3qIYrN5PH0W2GePRgUd2pBw5bNNA_dYMkjBd_oolJo"; // Hanya gunakan ID
    const apiKey = "AIzaSyD-mzIC0o3iGZKBor0_tYTodHwtbZgv_C4"; // API Key harus valid
    const range = "Sheet1!A:A"; // Pastikan ini sesuai dengan sheet dan kolom nama peserta

    let participants = [];

    async function fetchParticipants() {
        const url = `https://sheets.googleapis.com/v4/spreadsheets/${sheetId}/values/${range}?key=${apiKey}`;
        try {
            const response = await fetch(url);
            const data = await response.json();
            participants = data.values ? data.values.flat() : [];
            if (participants.length > 0) {
                initializeWheel();
            } else {
                console.error("No participants found in the spreadsheet.");
            }
        } catch (error) {
            console.error("Error fetching participants:", error);
        }
    }

    function initializeWheel() {
        window.wheel = new Winwheel({
            'numSegments': participants.length,
            'outerRadius': 200,
            'textFontSize': 18,
            'segments': participants.map(name => ({ 'fillStyle': getRandomColor(), 'text': name })),
            'animation': {
                'type': 'spinToStop',
                'duration': 5,
                'spins': 10,
                'callbackFinished': alertWinner
            }
        });
    }

    function getRandomColor() {
        return '#' + Math.floor(Math.random()*16777215).toString(16);
    }

    function startSpin() {
        if (participants.length === 0) {
            alert("Tidak ada peserta yang tersedia.");
            return;
        }
        wheel.startAnimation();
    }

    function alertWinner(indicatedSegment) {
        document.getElementById("winner").innerText = "Pemenang: " + indicatedSegment.text;
    }

    fetchParticipants(); // Ambil peserta dari Google Sheets saat halaman dimuat
</script>
