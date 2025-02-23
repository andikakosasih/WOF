async function fetchParticipants() {
    const url = `https://sheets.googleapis.com/v4/spreadsheets/${sheetId}/values/${range}?key=${apiKey}`;
    try {
        const response = await fetch(url);
        const data = await response.json();
        console.log("Data fetched:", data); // Debugging log
        participants = data.values ? data.values.flat() : [];
        if (participants.length > 0) {
            initializeWheel();
        } else {
            console.error("No participants found.");
        }
    } catch (error) {
        console.error("Error fetching participants:", error);
    }
}
