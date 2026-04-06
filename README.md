# spiritualgifts
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Spiritual Gifts Survey</title>
    <style>
        body { font-family: sans-serif; line-height: 1.6; max-width: 800px; margin: 20px auto; padding: 20px; background: #f4f4f9; }
        .card { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); margin-bottom: 20px; }
        h1, h2 { color: #2c3e50; }
        .instruction { font-style: italic; color: #555; margin-bottom: 20px; }
        .question { margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px solid #eee; }
        .options { display: flex; gap: 10px; margin-top: 5px; }
        label { cursor: pointer; }
        button { background: #3498db; color: white; border: none; padding: 10px 20px; border-radius: 5px; cursor: pointer; font-size: 16px; }
        button:hover { background: #2980b9; }
        #results { display: none; margin-top: 30px; }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        th, td { border: 1px solid #ddd; padding: 10px; text-align: left; }
        th { background: #f8f9fa; }
        .score-bar { background: #3498db; height: 20px; border-radius: 3px; }
    </style>
</head>
<body>

<div class="card">
    <h1>Spiritual Gifts Survey</h1>
    <p class="instruction">This is not a test. Select the response you feel best characterizes yourself[cite: 3, 5].</p>
    <p>
        <strong>5</strong> - Highly characteristic / Definitely true<br>
        <strong>4</strong> - Most of the time true<br>
        <strong>3</strong> - Frequently true (about 50%)<br>
        <strong>2</strong> - Occasionally true (about 25%)<br>
        <strong>1</strong> - Not at all characteristic [cite: 13-17]
    </p>
</div>

<form id="giftForm">
    <div id="questionsContainer" class="card">
        </div>
    <button type="button" onclick="calculateScores()">Calculate Results</button>
</form>

<div id="results" class="card">
    <h2>Your Spiritual Gifts Profile</h2>
    <p>Scores range from 5 to 25. Higher bars indicate your strongest gifts[cite: 130].</p>
    <table id="resultsTable">
        <thead>
            <tr>
                <th>Spiritual Gift</th>
                <th>Total Score</th>
                <th>Strength Visualization</th>
            </tr>
        </thead>
        <tbody></tbody>
    </table>
</div>

<script>
    const questions = [
        "I have the ability to organize ideas, resources, time, and people effectively.",
        "I am willing to study and prepare for the task of teaching.",
        "I am able to relate the truths of God to specific situations.",
        "I have a God-given ability to help others grow in their faith.",
        "I possess a special ability to communicate the truth of salvation.",
        "I have the ability to make critical decisions when necessary.",
        "I am sensitive to the hurts of people.",
        "I experience joy in meeting needs through sharing possessions.",
        "I enjoy studying.",
        "I have delivered God's message of warning and judgment.",
        "I am able to sense the true motivation of persons and movements.",
        "I have a special ability to trust God in difficult situations.",
        "I have a strong desire to contribute to the establishment of new churches.",
        "I take action to meet physical and practical needs rather than merely talking about or planning how to help.",
        "I enjoy entertaining guests in my home.",
        "I can adapt my guidance to fit the maturity of those working with me.",
        "I can delegate and assign meaningful work.",
        "I have an ability and desire to teach.",
        "I am usually able to analyze a situation correctly.",
        "I have a natural tendency to encourage others.",
        "I am willing to take the initiative in helping other Christians grow in their faith.",
        "I have an acute awareness of other people's emotions, such as loneliness, pain, fear, and anger.",
        "I am a cheerful giver.",
        "I spend time digging into facts.",
        "I feel that I have a message from God to deliver to others.",
        "I can recognize when a person is genuine/honest.",
        "I am a person of vision (a clear mental portrait of a preferable future given by God). I am able to communicate vision in such a way that others commit to making the vision a reality.",
        "I am willing to yield to God's will rather than question and waver.",
        "I would like to be more active in getting the gospel to people in other countries.",
        "It makes me happy to do things for people in need.",
        "I am successful in getting a group to do its work joyfully.",
        "I am able to make strangers feel at ease.",
        "I have the ability to teach to a variety of different learning styles.",
        "I can identify those who need encouragement.",
        "I have trained Christians to be more obedient disciples of Christ.",
        "I am willing to do whatever it takes to see others come to Christ.",
        "I am drawn to people who are hurting.",
        "I am a generous giver.",
        "I am able to discover new truths in Scripture.",
        "I have spiritual insights from Scripture concerning issues and people that compel me to speak out.",
        "I can sense when a person is acting in accordance with God's will.",
        "I can trust in God even when things look dark.",
        "I can determine where God wants a group to go and help it get there.",
        "I have a strong desire to take the gospel to places where it has never been heard.",
        "I enjoy reaching out to new people in my church and community.",
        "I am sensitive to the needs of people.",
        "I have been able to make effective and efficient plans for accomplishing the goals of a group.",
        "I often am consulted when fellow Christians are struggling to make difficult decisions.",
        "I think about how I can comfort and encourage others in my congregation.",
        "I am able to give spiritual direction to others.",
        "I am able to present the gospel to lost persons in such a way that they accept the Lord and His salvation.",
        "I possess an unusual capacity to understand the feelings of those in distress.",
        "I have a strong sense of stewardship based on the recognition that God owns all things.",
        "I have delivered to other persons messages that have come directly from God.",
        "I can sense when a person is acting under God's leadership.",
        "I try to be in God's will continually and be available for His use.",
        "I feel that I should take the gospel to people who have different beliefs from me.",
        "I have an acute awareness of the physical needs of others.",
        "I am skilled in setting forth positive and precise steps of action.",
        "I like to meet visitors at church and make them feel welcome.",
        "I explain Scripture in such a way that others understand it.",
        "I can usually see spiritual solutions to problems.",
        "I welcome opportunities to help people who need comfort, consolation, encouragement, and counseling.",
        "I feel at ease in sharing Christ with nonbelievers.",
        "I can influence others to perform to their highest God-given potential.",
        "I recognize the signs of stress and distress in others.",
        "I desire to give generously and unpretentiously to worthwhile projects and ministries.",
        "I can organize facts into meaningful relationships.",
        "God gives me messages to deliver to His people.",
        "I am able to sense whether people are being honest when they tell of their religious experiences.",
        "I enjoy presenting the gospel to persons of other cultures and backgrounds.",
        "I enjoy doing little things that help people.",
        "I can give a clear, uncomplicated presentation of the gospel.",
        "I have been able to apply biblical truth to the specific needs of my church.",
        "God has used me to encourage others to live Christlike lives.",
        "I have sensed the need to help other people become more effective in their ministries.",
        "I like to talk about Jesus to those who do not know Him.",
        "I have the ability to make strangers feel comfortable in my home.",
        "I have a wide range of study resources and know how to secure information.",
        "I feel assured that a situation will change for the glory of God even when the situation seem impossible."
    ];

    const mapping = {
        "Leadership": [6, 16, 27, 43, 65],
        "Administration": [1, 17, 31, 47, 59],
        "Teaching": [2, 18, 33, 61, 73],
        "Knowledge": [9, 24, 39, 68, 79],
        "Wisdom": [3, 19, 48, 62, 74],
        "Prophecy": [10, 25, 40, 54, 69],
        "Discernment": [11, 26, 41, 55, 70],
        "Exhortation": [20, 34, 49, 63, 75],
        "Shepherding": [4, 21, 35, 50, 76],
        "Faith": [12, 28, 42, 56, 80],
        "Evangelism": [5, 36, 51, 64, 77],
        "Apostleship": [13, 29, 44, 57, 71],
        "Service/Helps": [14, 30, 46, 58, 72],
        "Mercy": [7, 22, 37, 52, 66],
        "Giving": [8, 23, 38, 53, 67],
        "Hospitality": [15, 32, 45, 60, 78]
    };

    const container = document.getElementById('questionsContainer');
    questions.forEach((q, i) => {
        const div = document.createElement('div');
        div.className = 'question';
        div.innerHTML = `
            <p>${i + 1}. ${q}</p>
            <div class="options">
                ${[1, 2, 3, 4, 5].map(num => `
                    <label><input type="radio" name="q${i+1}" value="${num}" required> ${num}</label>
                `).join('')}
            </div>
        `;
        container.appendChild(div);
    });

    function calculateScores() {
        const formData = new FormData(document.getElementById('giftForm'));
        const resultsBody = document.querySelector('#resultsTable tbody');
        resultsBody.innerHTML = '';
        
        for (const [gift, items] of Object.entries(mapping)) {
            let total = 0;
            items.forEach(itemNum => {
                const val = formData.get(`q${itemNum}`);
                total += val ? parseInt(val) : 0;
            });

            const row = `
                <tr>
                    <td><strong>${gift}</strong></td>
                    <td>${total}</td>
                    <td><div class="score-bar" style="width: ${(total/25)*100}%"></div></td>
                </tr>
            `;
            resultsBody.insertAdjacentHTML('beforeend', row);
        }
        document.getElementById('results').style.display = 'block';
        window.scrollTo(0, document.getElementById('results').offsetTop);
    }
</script>

</body>
</html>
