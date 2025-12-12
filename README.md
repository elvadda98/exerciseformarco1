<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ESL Homework: AI Sentiment Shift</title>
    <style>
        :root {
            --primary-color: #007bff;
            --secondary-color: #6c757d;
            --success-color: #28a745;
            --danger-color: #dc3545;
            --warning-color: #ffc107;
            --background-color: #f8f9fa;
            --card-background: #ffffff;
            --text-color: #343a40;
            --border-color: #dee2e6;
            --transition-time: 0.3s;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
            background-color: var(--background-color);
            color: var(--text-color);
        }

        .container {
            max-width: 800px;
            margin: 20px auto;
            padding: 0 15px;
        }

        /* --- Slide System --- */
        .slides-wrapper {
            position: relative;
            overflow: hidden;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            background-color: var(--card-background);
            min-height: 60vh; /* Ensure some height for content */
            padding-bottom: 80px; /* Space for fixed controls */
        }

        .slide {
            padding: 30px;
            opacity: 0;
            transition: opacity var(--transition-time) ease-in-out;
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            box-sizing: border-box;
            overflow-y: auto;
        }

        .slide.active {
            opacity: 1;
            position: relative; /* Bring active slide to flow */
            z-index: 1;
        }

        /* Fix for scrollable content inside slide */
        .slides-container {
            position: relative;
        }

        h1 {
            color: var(--primary-color);
            border-bottom: 2px solid var(--border-color);
            padding-bottom: 10px;
            margin-top: 0;
            margin-bottom: 20px;
        }

        h2 {
            color: var(--secondary-color);
            margin-top: 20px;
            font-size: 1.5em;
        }

        p {
            line-height: 1.6;
            margin-bottom: 15px;
        }

        /* --- Controls --- */
        .controls-fixed {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            background-color: var(--card-background);
            border-top: 1px solid var(--border-color);
            box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.05);
            padding: 10px 0;
            display: flex;
            justify-content: space-around;
            align-items: center;
            z-index: 10;
        }

        .controls-fixed .container {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .slide-dots {
            display: flex;
            gap: 8px;
        }

        .dot {
            height: 10px;
            width: 10px;
            background-color: #bbb;
            border-radius: 50%;
            display: inline-block;
            cursor: pointer;
            transition: background-color var(--transition-time);
        }

        .dot.active {
            background-color: var(--primary-color);
        }

        .btn {
            background-color: var(--primary-color);
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color var(--transition-time);
        }

        .btn:hover:not(:disabled) {
            background-color: #0056b3;
        }

        .btn:disabled {
            background-color: var(--secondary-color);
            cursor: not-allowed;
            opacity: 0.6;
        }

        .btn-secondary {
            background-color: var(--secondary-color);
        }
        .btn-secondary:hover:not(:disabled) {
            background-color: #5a6268;
        }
        .btn-warning {
            background-color: var(--warning-color);
            color: var(--text-color);
        }
        .btn-warning:hover:not(:disabled) {
            background-color: #e0a800;
        }


        /* --- Exercise Formatting --- */
        .exercise-item {
            margin-bottom: 25px;
            padding: 15px;
            border: 1px solid var(--border-color);
            border-radius: 5px;
            background-color: #fafafa;
        }

        .feedback {
            margin-top: 10px;
            padding: 8px;
            border-radius: 4px;
            font-weight: bold;
            display: none;
        }

        .feedback.correct {
            background-color: #d4edda;
            color: var(--success-color);
            display: block;
        }

        .feedback.incorrect {
            background-color: #f8d7da;
            color: var(--danger-color);
            display: block;
        }

        .mc-option {
            display: block;
            margin: 8px 0;
            padding: 10px;
            border: 1px solid var(--border-color);
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.1s;
        }

        .mc-option:hover {
            background-color: #f0f0f0;
        }

        .mc-option.selected {
            border-color: var(--primary-color);
            background-color: #e9f0ff;
        }

        .mc-option.mc-correct {
            background-color: #d4edda;
            border-color: var(--success-color);
            font-weight: bold;
        }

        .mc-option.mc-incorrect {
            background-color: #f8d7da;
            border-color: var(--danger-color);
        }

        input[type="text"], textarea, select, input[type="number"] {
            padding: 8px;
            border: 1px solid var(--border-color);
            border-radius: 4px;
            width: 100%;
            box-sizing: border-box;
            margin-top: 5px;
            transition: border-color var(--transition-time), background-color var(--transition-time);
        }

        input[type="text"].correct, select.correct, input[type="number"].correct {
            border-color: var(--success-color);
            background-color: #e6ffe6;
        }

        input[type="text"].incorrect, select.incorrect, input[type="number"].incorrect {
            border-color: var(--danger-color);
            background-color: #ffe6e6;
        }

        input[type="text"].empty, select.empty, input[type="number"].empty {
            border-color: var(--warning-color);
            background-color: #fff9e6;
        }

        .word-bank {
            border: 1px dashed var(--primary-color);
            padding: 10px;
            margin-bottom: 20px;
            border-radius: 4px;
            background-color: #e9f0ff;
        }

        .pron-feedback {
            margin-top: 5px;
            font-size: 0.9em;
            padding: 5px 8px;
            border-radius: 4px;
            min-height: 30px;
        }

        .tts-button {
            margin-left: 10px;
            padding: 5px 10px;
            background-color: var(--primary-color);
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 0.9em;
        }

        .recognition-status {
            margin-top: 10px;
            font-style: italic;
            color: var(--secondary-color);
        }
    </style>
</head>
<body>

<div class="container">
    <div class="slides-wrapper">
        <div class="slides-container">

            <div class="slide active" data-slide="1">
                <h1>📰 OpenAI Goes From Stock Market Savior to Burden as AI Risks Mount</h1>
                <p><strong>2025-12-07 14:00:07 GMT</strong><br>By Ryan Vlastelica (Bloomberg)</p>
                <p>Wall Street’s sentiment toward companies associated with artificial intelligence is shifting, and it’s all about two companies: **OpenAI is down, and Alphabet Inc. is up.**</p>
                <p>The maker of ChatGPT is no longer seen as being on the cutting edge of AI technology and is facing questions about its lack of profitability and the need to grow rapidly to pay for its massive spending commitments. Meanwhile, Google’s parent is emerging as a deep-pocketed competitor with **tentacles** in every part of the AI trade.</p>
                <p>“OpenAI was the golden child earlier this year, and Alphabet was looked at in a very different light,” said Brett Ewing, chief market strategist at First Franklin Financial Services. “Now sentiment is much more **tempered** toward OpenAI.” </p>
                <p>As a result, the shares of companies in OpenAI’s orbit — principally Oracle Corp., CoreWeave Inc., and Advanced Micro Devices Inc., but also Microsoft Corp., Nvidia Corp. and SoftBank, which has an 11% stake in the company — are coming under heavy selling pressure. Meanwhile, Alphabet’s **momentum** is boosting not only its stock price, but also those it’s associated with like Broadcom Inc., Lumentum Holdings Inc., Celestica Inc., and TTM Technologies Inc.</p>
                <p>The shift has been dramatic in magnitude and speed. Just a few weeks ago, OpenAI was **sparking** huge rallies in any company related to it. Now, those connections look more like an **anchor**. It’s a change that carries wide-ranging implications, given how central the closely held company has been to the AI **mania** that has driven the stock market’s three-year rally. </p>
                <p>“A light has been shined on the complexity of the financing, the circular deals, the debt issues,” Ewing said. “I’m sure this exists around the Alphabet **ecosystem** to a certain degree, but it was exposed as pretty extreme for OpenAI’s deals, and appreciating that was a game-changer for sentiment.”</p>
                <p>A basket of companies connected to OpenAI has gained 74% in 2025, which is impressive but far shy of the 146% jump by Alphabet-exposed stocks. The technology-heavy Nasdaq 100 Index is up 22%. </p>
                <p>The **skepticism** surrounding OpenAI can be dated to August, when it unveiled GPT-5 to mixed reactions. It ramped up last month when Alphabet released the latest version of it Gemini AI model and got rave reviews. As a result, OpenAI Chief Executive Officer Sam Altman declared a “code red” effort to improve the quality of ChatGPT, delaying other projects until it gets its signature product in line.</p>
                <p>Alphabet’s perceived strength goes beyond Gemini. The company has the third highest market capitalization in the S&P 500 and a ton of cash at its disposal. It also has host of adjacent businesses, like Google Cloud and a semiconductor manufacturing operation that’s gaining **traction**. And that’s before you consider the company’s AI data, talent and distribution, or its successful subsidiaries like YouTube and Waymo.</p>
                <p>“There’s a growing sense that Alphabet has all the pieces to emerge as the **dominant** AI model builder,” said Brian Colello, technology equity senior strategist at Morningstar. “Just a couple months ago, investors would’ve given that title to OpenAI. Now there’s more uncertainty, more competition, more risk that OpenAI isn’t the slam-dunk winner.”</p>
                <p>The difference between being first or second place goes beyond bragging rights, it also has significant financial ramifications for the companies and their partners. For example, if users gravitating to Gemini slows ChatGPT’s growth, it will be harder for OpenAI to pay for cloud-computing capacity from Oracle or chips from AMD.</p>
                <p>By contrast, Alphabet’s partners in building out its AI effort are **thriving**. Shares of Lumentum, which makes optical components for Alphabet’s data centers, have more than tripled this year... Celestica provides the hardware for Alphabet’s AI buildout, and its stock is up 252% in 2025. Meanwhile Broadcom — which is building the tensor processing unit, or TPU, chips Alphabet uses — has seen its stock price leap 68% since the end of last year.</p>
                <p>OpenAI has announced a number of ambitious deals in recent months. The flurry of activity “rightfully brought **scrutiny** and concern over whether OpenAI can fund all this, whether it is biting off more than it can chew,” Colello said. “The timing of its revenue growth is uncertain, and every improvement a competitor makes adds to the risk that it can’t reach its aspirations.”</p>
                <p>In fairness, investors greeted many of these deals with excitement, because they appeared to mint the next generation of AI winners. But with the shift in sentiment, they’re suddenly taking a wait-and-see attitude.</p>
                <p>“When people thought it could generate revenue and become profitable, those big deal numbers seemed possible,” said Brian Kersmanc, portfolio manager at GQG Partners, which has about $160 billion in assets. “Now we’re at a point where people have stopped believing and started questioning.”</p>
                <p>Kersmanc sees the AI **euphoria** as the “dot-com era on steroids,” and said his firm has gone from being heavily overweight tech to highly skeptical.</p>
            </div>

            <div class="slide" data-slide="2">
                <h2>🧐 Reading Comprehension Check</h2>
                <p>Read the article on Slide 1 and choose the best answer for each question.</p>
                <form id="mcq-form">
                    <div class="exercise-item" data-correct-option="C">
                        <p><strong>1. According to the article, what is the main shift in Wall Street's sentiment?</strong></p>
                        <label class="mc-option"><input type="radio" name="q1" value="A"> A. Investors are ignoring AI companies completely.</label>
                        <label class="mc-option"><input type="radio" name="q1" value="B"> B. Both OpenAI and Alphabet are losing investor confidence.</label>
                        <label class="mc-option"><input type="radio" name="q1" value="C"> C. Confidence in OpenAI is decreasing while confidence in Alphabet is increasing.</label>
                        <label class="mc-option"><input type="radio" name="q1" value="D"> D. Alphabet's stock is falling due to increased competition from OpenAI.</label>
                        <div class="feedback"></div>
                    </div>

                    <div class="exercise-item" data-correct-option="B">
                        <p><strong>2. What is one of the primary concerns for investors regarding OpenAI?</strong></p>
                        <label class="mc-option"><input type="radio" name="q2" value="A"> A. It has stopped making new deals with partners like Oracle.</label>
                        <label class="mc-option"><input type="radio" name="q2" value="B"> B. Its profitability and ability to fund massive spending commitments are uncertain.</label>
                        <label class="mc-option"><input type="radio" name="q2" value="C"> C. Its CEO Sam Altman announced he is leaving the company.</label>
                        <label class="mc-option"><input type="radio" name="q2" value="D"> D. It is heavily reliant on the US government for funding.</label>
                        <div class="feedback"></div>
                    </div>

                    <div class="exercise-item" data-correct-option="A">
                        <p><strong>3. Which event contributed to the growing skepticism about OpenAI, leading to a "code red" effort?</strong></p>
                        <label class="mc-option"><input type="radio" name="q3" value="A"> A. Alphabet released its Gemini AI model to rave reviews.</label>
                        <label class="mc-option"><input type="radio" name="q3" value="B"> B. OpenAI's stock gained 74% in 2025.</label>
                        <label class="mc-option"><input type="radio" name="q3" value="C"> C. The Nasdaq 100 Index went up 22%.</label>
                        <label class="mc-option"><input type="radio" name="q3" value="D"> D. Oracle Corp. pulled out of a deal with OpenAI.</label>
                        <div class="feedback"></div>
                    </div>

                    <div class="exercise-item" data-correct-option="D">
                        <p><strong>4. Alphabet is seen as a dominant competitor partly because of its "tentacles" in the AI trade, which includes all of the following EXCEPT:</strong></p>
                        <label class="mc-option"><input type="radio" name="q4" value="A"> A. Significant cash at its disposal and high market capitalization.</label>
                        <label class="mc-option"><input type="radio" name="q4" value="B"> B. Adjacent businesses like Google Cloud.</label>
                        <label class="mc-option"><input type="radio" name="q4" value="C"> C. Successful subsidiaries like YouTube and Waymo.</label>
                        <label class="mc-option"><input type="radio" name="q4" value="D"> D. A history of highly successful, widely adopted new products released every quarter.</label>
                        <div class="feedback"></div>
                    </div>

                    <button type="button" class="btn" onclick="checkMCQ('mcq-form')">Check Answers</button>
                    <p><strong>Score: </strong><span id="mcq-score">0/4</span></p>
                </form>
            </div>

            <div class="slide" data-slide="3">
                <h2>🧠 Vocabulary in Context</h2>
                <p>Choose the option that best explains the meaning of the **bold** word/phrase as it is used in the article.</p>
                <form id="vocab-context-form">

                    <div class="exercise-item" data-correct-option="C">
                        <p><strong>1. "OpenAI was the golden child earlier this year..."</strong></p>
                        <label class="mc-option"><input type="radio" name="qV1" value="A"> A. A company that makes products for children.</label>
                        <label class="mc-option"><input type="radio" name="qV1" value="B"> B. A company that is very old and respected.</label>
                        <label class="mc-option"><input type="radio" name="qV1" value="C"> C. A successful person or thing that is admired by others.</label>
                        <label class="mc-option"><input type="radio" name="qV1" value="D"> D. A company that has a lot of gold reserves.</label>
                        <div class="feedback"></div>
                    </div>

                    <div class="exercise-item" data-correct-option="D">
                        <p><strong>2. "...sentiment is much more tempered toward OpenAI.”</strong></p>
                        <label class="mc-option"><input type="radio" name="qV2" value="A"> A. More enthusiastic and positive.</label>
                        <label class="mc-option"><input type="radio" name="qV2" value="B"> B. Hot and angry.</label>
                        <label class="mc-option"><input type="radio" name="qV2" value="C"> C. Very cold and unfriendly.</label>
                        <label class="mc-option"><input type="radio" name="qV2" value="D"> D. More moderate, controlled, or restrained.</label>
                        <div class="feedback"></div>
                    </div>

                    <div class="exercise-item" data-correct-option="A">
                        <p><strong>3. "...a deep-pocketed competitor with tentacles in every part of the AI trade."</strong></p>
                        <label class="mc-option"><input type="radio" name="qV3" value="A"> A. Different sections or areas of a large, influential organization.</label>
                        <label class="mc-option"><input type="radio" name="qV3" value="B"> B. Physical arms of a sea creature.</label>
                        <label class="mc-option"><input type="radio" name="qV3" value="C"> C. New types of technology being developed.</label>
                        <label class="mc-option"><input type="radio" name="qV3" value="D"> D. Legal contracts and agreements.</label>
                        <div class="feedback"></div>
                    </div>

                    <div class="exercise-item" data-correct-option="B">
                        <p><strong>4. "...OpenAI was sparking huge rallies in any company related to it."</strong></p>
                        <label class="mc-option"><input type="radio" name="qV4" value="A"> A. Causing large outdoor public meetings.</label>
                        <label class="mc-option"><input type="radio" name="qV4" value="B"> B. Causing quick and strong increases in stock prices.</label>
                        <label class="mc-option"><input type="radio" name="qV4" value="C"> C. Lighting small fires in many places.</label>
                        <label class="mc-option"><input type="radio" name="qV4" value="D"> D. Encouraging people to buy small amounts of stock.</label>
                        <div class="feedback"></div>
                    </div>

                    <div class="exercise-item" data-correct-option="C">
                        <p><strong>5. "The company’s semiconductor manufacturing operation that’s gaining traction."</strong></p>
                        <label class="mc-option"><input type="radio" name="qV5" value="A"> A. Losing speed or becoming unpopular.</label>
                        <label class="mc-option"><input type="radio" name="qV5" value="B"> B. Making a lot of loud noise.</label>
                        <label class="mc-option"><input type="radio" name="qV5" value="C"> C. Starting to become popular, accepted, or successful.</label>
                        <label class="mc-option"><input type="radio" name="qV5" value="D"> D. Getting stuck in the mud.</label>
                        <div class="feedback"></div>
                    </div>

                    <button type="button" class="btn" onclick="checkMCQ('vocab-context-form')">Check Answers</button>
                    <p><strong>Score: </strong><span id="vocab-context-score">0/5</span></p>
                </form>
            </div>

            <div class="slide" data-slide="4">
                <h2>📝 Cloze Activity</h2>
                <p>Complete the short paragraph below with the correct words from the word bank. **The order of the words below is shuffled.**</p>
                <div class="word-bank">
                    <p><strong>Choose from:</strong> <span id="wb4"></span></p>
                </div>
                <div class="exercise-item">
                    <p>The investor <input type="text" id="cloze1" data-correct="sentiment" maxlength="15" class="cloze-input"> toward OpenAI has shifted. The company, once a "golden child," is now facing <input type="text" id="cloze2" data-correct="skepticism" maxlength="15" class="cloze-input"> due to concerns over its financing and massive spending. In contrast, Google's parent, Alphabet, is gaining <input type="text" id="cloze3" data-correct="momentum" maxlength="15" class="cloze-input"> and is perceived as the more <input type="text" id="cloze4" data-correct="dominant" maxlength="15" class="cloze-input"> competitor in the AI market.</p>
                </div>
                <button type="button" class="btn" onclick="checkCloze()">Check Answers</button>
                <p><strong>Score: </strong><span id="cloze-score">0/4</span></p>
            </div>

            <div class="slide" data-slide="5">
                <h2>🔗 Matching Vocabulary</h2>
                <p>Match the key vocabulary word from the article with its definition.</p>
                <form id="matching-form">
                    <div class="exercise-item matching-item">
                        <p>1. **Skepticism**</p>
                        <select id="match1" data-correct="D">
                            <option value="">-- Select Definition --</option>
                            <option value="A">A feeling of great happiness or excitement.</option>
                            <option value="B">The main, most important, or powerful one.</option>
                            <option value="C">Intense public attention, or a thorough examination.</option>
                            <option value="D">A doubtful attitude or belief that claims must be questioned.</option>
                            <option value="E">An object used to keep a boat in place, or a person/thing that provides stability.</option>
                        </select>
                    </div>

                    <div class="exercise-item matching-item">
                        <p>2. **Anchor**</p>
                        <select id="match2" data-correct="E">
                            <option value="">-- Select Definition --</option>
                            <option value="A">A feeling of great happiness or excitement.</option>
                            <option value="B">The main, most important, or powerful one.</option>
                            <option value="C">Intense public attention, or a thorough examination.</option>
                            <option value="D">A doubtful attitude or belief that claims must be questioned.</option>
                            <option value="E">A person or thing providing stability or security (negative context here: a weight/burden).</option>
                        </select>
                    </div>

                    <div class="exercise-item matching-item">
                        <p>3. **Scrutiny**</p>
                        <select id="match3" data-correct="C">
                            <option value="">-- Select Definition --</option>
                            <option value="A">A feeling of great happiness or excitement.</option>
                            <option value="B">The main, most important, or powerful one.</option>
                            <option value="C">Intense public attention or a thorough, close examination.</option>
                            <option value="D">A doubtful attitude or belief that claims must be questioned.</option>
                            <option value="E">A person or thing providing stability or security (negative context here: a weight/burden).</option>
                        </select>
                    </div>

                    <div class="exercise-item matching-item">
                        <p>4. **Euphoria**</p>
                        <select id="match4" data-correct="A">
                            <option value="">-- Select Definition --</option>
                            <option value="A">A feeling of great happiness or excitement.</option>
                            <option value="B">The main, most important, or powerful one.</option>
                            <option value="C">Intense public attention, or a thorough examination.</option>
                            <option value="D">A doubtful attitude or belief that claims must be questioned.</option>
                            <option value="E">A person or thing providing stability or security (negative context here: a weight/burden).</option>
                        </select>
                    </div>

                    <div class="exercise-item matching-item">
                        <p>5. **Dominant**</p>
                        <select id="match5" data-correct="B">
                            <option value="">-- Select Definition --</option>
                            <option value="A">A feeling of great happiness or excitement.</option>
                            <option value="B">The main, most important, or powerful one; exercising control.</option>
                            <option value="C">Intense public attention, or a thorough examination.</option>
                            <option value="D">A doubtful attitude or belief that claims must be questioned.</option>
                            <option value="E">A person or thing providing stability or security (negative context here: a weight/burden).</option>
                        </select>
                    </div>

                    <button type="button" class="btn" onclick="checkMatching()">Check Answers</button>
                    <p><strong>Score: </strong><span id="matching-score">0/5</span></p>
                </form>
            </div>

            <div class="slide" data-slide="6">
                <h2>✍️ Production: Writing Prompts</h2>
                <p>Use the following words/phrases from the article to write one sentence for each prompt. Try to use them in a new context.</p>

                <div class="exercise-item">
                    <p><strong>1. Use the phrase: 'biting off more than it can chew'</strong></p>
                    <textarea rows="3" placeholder="Your sentence here..."></textarea>
                </div>

                <div class="exercise-item">
                    <p><strong>2. Use the word: 'tempered' (as an adjective or verb)</strong></p>
                    <textarea rows="3" placeholder="Your sentence here..."></textarea>
                </div>

                <div class="exercise-item">
                    <p><strong>3. Use the word: 'ecosystem'</strong></p>
                    <textarea rows="3" placeholder="Your sentence here..."></textarea>
                </div>

                <p class="note" style="color: var(--danger-color); font-style: italic;">Note: Your teacher will review these answers.</p>
            </div>

            <div class="slide" data-slide="7">
                <h2>🔢 Reorder Events (Chronology)</h2>
                <p>The sentences below summarize key events from the article. Put them in the correct chronological order (1 is the earliest event, 5 is the most recent or final result).</p>
                <form id="reorder-form">
                    <div id="reorder-items">
                        <div class="exercise-item reorder-item" data-order="3">
                            <label for="reorder1">Enter order (1-5):</label>
                            <input type="number" id="reorder1" min="1" max="5" class="reorder-input">
                            <p>Alphabet released its Gemini AI model and received rave reviews.</p>
                        </div>
                        <div class="exercise-item reorder-item" data-order="5">
                            <label for="reorder2">Enter order (1-5):</label>
                            <input type="number" id="reorder2" min="1" max="5" class="reorder-input">
                            <p>OpenAI's CEO declared a "code red" to improve ChatGPT's quality, delaying other projects.</p>
                        </div>
                        <div class="exercise-item reorder-item" data-order="1">
                            <label for="reorder3">Enter order (1-5):</label>
                            <input type="number" id="reorder3" min="1" max="5" class="reorder-input">
                            <p>OpenAI was seen as the "golden child" and was sparking huge stock market rallies.</p>
                        </div>
                        <div class="exercise-item reorder-item" data-order="2">
                            <label for="reorder4">Enter order (1-5):</label>
                            <input type="number" id="reorder4" min="1" max="5" class="reorder-input">
                            <p>OpenAI unveiled GPT-5, which was met with mixed reactions from the market.</p>
                        </div>
                        <div class="exercise-item reorder-item" data-order="4">
                            <label for="reorder5">Enter order (1-5):</label>
                            <input type="number" id="reorder5" min="1" max="5" class="reorder-input">
                            <p>Investor sentiment became much more tempered and skeptical toward OpenAI.</p>
                        </div>
                    </div>

                    <button type="button" class="btn" onclick="checkReorder()">Check Order</button>
                    <p><strong>Score: </strong><span id="reorder-score">0/5</span></p>
                </form>
            </div>

            <div class="slide" data-slide="8">
                <h2>🗣️ Pronunciation Practice A: Complete the Sentence</h2>
                <p>The missing word for each sentence is in the word bank below. **Say the entire missing word** into your microphone and press 'Record'.</p>
                <p class="recognition-status" id="pron-status-A">Speech Recognition status: Not checked.</p>
                <div class="word-bank">
                    <p><strong>Choose from:</strong> <span id="wb8"></span></p>
                </div>

                <div id="pronA-container">
                    <div class="exercise-item pron-item" data-target="profitability">
                        <p>Sentence: OpenAI is facing questions about its lack of ________________.</p>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'profitability')">Record</button>
                        <span class="pron-feedback"></span>
                    </div>
                    <div class="exercise-item pron-item" data-target="sentiment">
                        <p>Sentence: Wall Street’s ________________ toward AI companies is shifting.</p>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'sentiment')">Record</button>
                        <span class="pron-feedback"></span>
                    </div>
                    <div class="exercise-item pron-item" data-target="anchor">
                        <p>Sentence: For some, connections to OpenAI now look more like an ________________.</p>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'anchor')">Record</button>
                        <span class="pron-feedback"></span>
                    </div>
                    <div class="exercise-item pron-item" data-target="thriving">
                        <p>Sentence: Alphabet’s partners in building out its AI effort are ________________.</p>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'thriving')">Record</button>
                        <span class="pron-feedback"></span>
                    </div>
                    <div class="exercise-item pron-item" data-target="ramifications">
                        <p>Sentence: The difference between first and second place has significant financial ________________.</p>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'ramifications')">Record</button>
                        <span class="pron-feedback"></span>
                    </div>
                </div>
            </div>

            <div class="slide" data-slide="9">
                <h2>🗣️ Pronunciation Practice B: Say the Word</h2>
                <p>Say the vocabulary word that matches the definition below. The correct words are in the word bank (shuffled).</p>
                <p class="recognition-status" id="pron-status-B">Speech Recognition status: Not checked.</p>
                <div class="word-bank">
                    <p><strong>Choose from:</strong> <span id="wb9"></span></p>
                </div>

                <div id="pronB-container">
                    <div class="exercise-item pron-item" data-target="momentum">
                        <p>Definition: The force or speed of movement, or the continuous development of something.</p>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'momentum')">Record</button>
                        <span class="pron-feedback"></span>
                    </div>
                    <div class="exercise-item pron-item" data-target="tempered">
                        <p>Definition: Made less intense or severe; moderated or restrained.</p>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'tempered')">Record</button>
                        <span class="pron-feedback"></span>
                    </div>
                    <div class="exercise-item pron-item" data-target="mania">
                        <p>Definition: Extremely excessive enthusiasm or obsession.</p>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'mania')">Record</button>
                        <span class="pron-feedback"></span>
                    </div>
                    <div class="exercise-item pron-item" data-target="scrutiny">
                        <p>Definition: Critical observation or examination.</p>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'scrutiny')">Record</button>
                        <span class="pron-feedback"></span>
                    </div>
                    <div class="exercise-item pron-item" data-target="ecosystem">
                        <p>Definition: A complex network or interconnected system of things.</p>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'ecosystem')">Record</button>
                        <span class="pron-feedback"></span>
                    </div>
                </div>
            </div>

            <div class="slide" data-slide="10">
                <h2>👂 Pronunciation Practice C: Listen & Repeat</h2>
                <p>Listen to the word, then record yourself saying it. Aim for clear pronunciation.</p>
                <p class="recognition-status" id="pron-status-C">Speech Recognition status: Not checked.</p>

                <ol>
                    <li class="exercise-item pron-item" data-target="Alphabet">
                        <span style="font-weight: bold;">Alphabet</span>
                        <button type="button" class="tts-button" onclick="speakWord('Alphabet')">Listen</button>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'Alphabet')">Record</button>
                        <span class="pron-feedback"></span>
                    </li>
                    <li class="exercise-item pron-item" data-target="dominant">
                        <span style="font-weight: bold;">Dominant</span>
                        <button type="button" class="tts-button" onclick="speakWord('Dominant')">Listen</button>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'dominant')">Record</button>
                        <span class="pron-feedback"></span>
                    </li>
                    <li class="exercise-item pron-item" data-target="traction">
                        <span style="font-weight: bold;">Traction</span>
                        <button type="button" class="tts-button" onclick="speakWord('Traction')">Listen</button>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'traction')">Record</button>
                        <span class="pron-feedback"></span>
                    </li>
                    <li class="exercise-item pron-item" data-target="euphoria">
                        <span style="font-weight: bold;">Euphoria</span>
                        <button type="button" class="tts-button" onclick="speakWord('Euphoria')">Listen</button>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'euphoria')">Record</button>
                        <span class="pron-feedback"></span>
                    </li>
                    <li class="exercise-item pron-item" data-target="financing">
                        <span style="font-weight: bold;">Financing</span>
                        <button type="button" class="tts-button" onclick="speakWord('Financing')">Listen</button>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'financing')">Record</button>
                        <span class="pron-feedback"></span>
                    </li>
                    <li class="exercise-item pron-item" data-target="competitor">
                        <span style="font-weight: bold;">Competitor</span>
                        <button type="button" class="tts-button" onclick="speakWord('Competitor')">Listen</button>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'competitor')">Record</button>
                        <span class="pron-feedback"></span>
                    </li>
                    <li class="exercise-item pron-item" data-target="skepticism">
                        <span style="font-weight: bold;">Skepticism</span>
                        <button type="button" class="tts-button" onclick="speakWord('Skepticism')">Listen</button>
                        <button type="button" class="btn btn-secondary" onclick="startRecognitionPron(this, 'skepticism')">Record</button>
                        <span class="pron-feedback"></span>
                    </li>
                </ol>
            </div>

        </div> </div> <div class="controls-fixed">
        <div class="container">
            <button class="btn btn-secondary" id="prevBtn" onclick="prevSlide()" disabled>← Previous</button>
            <div style="text-align: center;">
                <p id="slide-label" style="margin: 5px 0 10px 0; font-weight: bold; font-size: 0.9em;">Slide 1 / 10</p>
                <div class="slide-dots" id="slideDots">
                    </div>
            </div>
            <button class="btn" id="nextBtn" onclick="nextSlide()">Next →</button>
        </div>
    </div>

</div> <script>
    let currentSlide = 1;
    const totalSlides = 10;
    const slides = document.querySelectorAll('.slide');
    const slideLabel = document.getElementById('slide-label');
    const prevBtn = document.getElementById('prevBtn');
    const nextBtn = document.getElementById('nextBtn');
    const slideDotsContainer = document.getElementById('slideDots');

    // --- Utility Functions ---

    // Fisher-Yates (Knuth) Shuffle
    function shuffleArray(array) {
        for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
        }
        return array;
    }

    function createWordBank(words, id) {
        const container = document.getElementById(id);
        if (!container) return;
        const shuffledWords = shuffleArray([...words]);
        container.textContent = shuffledWords.join(' | ');
    }

    function shuffleContainerChildren(containerId) {
        const container = document.getElementById(containerId);
        if (!container) return;
        const items = Array.from(container.children);
        const shuffledItems = shuffleArray(items);
        container.innerHTML = '';
        shuffledItems.forEach(item => container.appendChild(item));
    }

    // --- Slide Navigation Logic ---

    function updateControls() {
        slideLabel.textContent = `Slide ${currentSlide} / ${totalSlides}`;
        prevBtn.disabled = currentSlide === 1;
        nextBtn.disabled = currentSlide === totalSlides;

        const dots = slideDotsContainer.querySelectorAll('.dot');
        dots.forEach((dot, index) => {
            dot.classList.toggle('active', index + 1 === currentSlide);
        });
    }

    function showSlide(n) {
        if (n < 1 || n > totalSlides) return;

        // Hide all slides
        slides.forEach(slide => slide.classList.remove('active'));

        // Show the target slide
        const targetSlide = document.querySelector(`.slide[data-slide="${n}"]`);
        if (targetSlide) {
            targetSlide.classList.add('active');
            currentSlide = n;
            updateControls();
            // Ensure the main container scrolls to top when changing slides
            document.querySelector('.slides-wrapper').scrollTo({ top: 0, behavior: 'smooth' });
        }
    }

    function nextSlide() {
        showSlide(currentSlide + 1);
    }

    function prevSlide() {
        showSlide(currentSlide - 1);
    }

    function goToSlide(n) {
        showSlide(n);
    }

    function initializeDots() {
        for (let i = 1; i <= totalSlides; i++) {
            const dot = document.createElement('span');
            dot.classList.add('dot');
            dot.setAttribute('onclick', `goToSlide(${i})`);
            slideDotsContainer.appendChild(dot);
        }
        updateControls();
    }

    // --- Exercise Checkers ---

    function checkMCQ(formId) {
        const form = document.getElementById(formId);
        let correctCount = 0;
        const totalQuestions = form.querySelectorAll('.exercise-item').length;

        form.querySelectorAll('.exercise-item').forEach(item => {
            const correctOption = item.getAttribute('data-correct-option');
            const questionName = item.querySelector('input[type="radio"]').name;
            const selectedOption = form.querySelector(`input[name="${questionName}"]:checked`);
            const feedbackElement = item.querySelector('.feedback');

            // Reset classes
            item.querySelectorAll('.mc-option').forEach(opt => {
                opt.classList.remove('mc-correct', 'mc-incorrect', 'selected');
            });

            feedbackElement.style.display = 'none';

            if (selectedOption) {
                selectedOption.closest('.mc-option').classList.add('selected');

                if (selectedOption.value === correctOption) {
                    correctCount++;
                    selectedOption.closest('.mc-option').classList.add('mc-correct');
                    feedbackElement.textContent = "Correct!";
                    feedbackElement.className = 'feedback correct';
                } else {
                    selectedOption.closest('.mc-option').classList.add('mc-incorrect');
                    // Highlight the correct answer
                    item.querySelector(`input[value="${correctOption}"]`).closest('.mc-option').classList.add('mc-correct');
                    feedbackElement.textContent = `Incorrect. The correct answer is ${correctOption}.`;
                    feedbackElement.className = 'feedback incorrect';
                }
            } else {
                feedbackElement.textContent = "Please select an answer.";
                feedbackElement.className = 'feedback incorrect';
            }
        });

        document.getElementById(`${formId.split('-')[0]}-score`).textContent = `${correctCount}/${totalQuestions}`;
    }

    function checkCloze() {
        const inputs = document.querySelectorAll('.cloze-input');
        let correctCount = 0;
        const totalQuestions = inputs.length;

        inputs.forEach(input => {
            const correctWord = input.getAttribute('data-correct').toLowerCase().trim();
            const userAnswer = input.value.toLowerCase().trim();

            input.classList.remove('correct', 'incorrect', 'empty');

            if (userAnswer === '') {
                input.classList.add('empty');
            } else if (userAnswer === correctWord) {
                input.classList.add('correct');
                correctCount++;
            } else {
                input.classList.add('incorrect');
            }
        });

        document.getElementById('cloze-score').textContent = `${correctCount}/${totalQuestions}`;
    }

    function checkMatching() {
        const selects = document.querySelectorAll('#matching-form select');
        let correctCount = 0;
        const totalQuestions = selects.length;

        selects.forEach(select => {
            const correctOption = select.getAttribute('data-correct');
            const userAnswer = select.value;

            select.classList.remove('correct', 'incorrect', 'empty');

            if (userAnswer === '') {
                select.classList.add('empty');
            } else if (userAnswer === correctOption) {
                select.classList.add('correct');
                correctCount++;
            } else {
                select.classList.add('incorrect');
            }
        });

        document.getElementById('matching-score').textContent = `${correctCount}/${totalQuestions}`;
    }

    function checkReorder() {
        const items = document.querySelectorAll('.reorder-item');
        let correctCount = 0;
        const totalQuestions = items.length;

        items.forEach(item => {
            const correctOrder = item.getAttribute('data-order');
            const input = item.querySelector('.reorder-input');
            const userAnswer = input.value.trim();

            input.classList.remove('correct', 'incorrect', 'empty');

            if (userAnswer === '') {
                input.classList.add('empty');
            } else if (userAnswer === correctOrder) {
                input.classList.add('correct');
                correctCount++;
            } else {
                input.classList.add('incorrect');
            }
        });

        document.getElementById('reorder-score').textContent = `${correctCount}/${totalQuestions}`;
    }

    // --- Speech Recognition Logic ---

    let recognition = null;
    let targetWordGlobal = '';
    let feedbackElementGlobal = null;

    function initRecognition(statusId) {
        const statusElement = document.getElementById(statusId);
        if ('webkitSpeechRecognition' in window) {
            recognition = new webkitSpeechRecognition();
            recognition.continuous = false;
            recognition.interimResults = false;
            recognition.lang = 'en-US';

            recognition.onstart = () => {
                if (feedbackElementGlobal) {
                    feedbackElementGlobal.innerHTML = '<span style="color: var(--warning-color);">Listening... Say the word now.</span>';
                }
            };

            recognition.onerror = (event) => {
                console.error('Speech recognition error:', event);
                if (feedbackElementGlobal) {
                    feedbackElementGlobal.innerHTML = '<span style="color: var(--danger-color);">Error: Try again or check mic permissions.</span>';
                }
                recognition.stop();
            };

            recognition.onend = () => {
                if (feedbackElementGlobal && !feedbackElementGlobal.innerHTML.includes('Correct')) {
                     // Only show 'Try again' if not already successful
                    if (!feedbackElementGlobal.innerHTML.includes('Listening')) {
                        feedbackElementGlobal.innerHTML = '<span style="color: var(--secondary-color);">Recording finished. Try again.</span>';
                    }
                }
            };

            recognition.onresult = (event) => {
                const transcript = event.results[0][0].transcript.toLowerCase().trim();
                const target = targetWordGlobal.toLowerCase().trim();

                let isCorrect = (transcript === target);
                // Simple partial match for safety
                if (!isCorrect && transcript.includes(target)) {
                    isCorrect = true;
                }

                if (isCorrect) {
                    feedbackElementGlobal.innerHTML = `<span style="color: var(--success-color);">Correct! (You said: "${transcript}")</span>`;
                } else {
                    feedbackElementGlobal.innerHTML = `<span style="color: var(--danger-color);">Try again. (You said: "${transcript}")</span>`;
                }
                recognition.stop();
            };

            if (statusElement) statusElement.textContent = "Speech Recognition status: Ready. (Use Chrome/Edge for best results)";
            return recognition;
        } else {
            if (statusElement) statusElement.textContent = "Speech Recognition status: Not Supported. (Use Chrome or Edge browser.)";
            return null;
        }
    }

    function startRecognitionPron(button, targetWord) {
        if (!recognition) {
            alert("Speech recognition is not supported in this browser. Please use Chrome or Edge.");
            return;
        }

        const pronItem = button.closest('.pron-item');
        feedbackElementGlobal = pronItem.querySelector('.pron-feedback');
        targetWordGlobal = targetWord;

        try {
            recognition.start();
        } catch (e) {
            console.error('Recognition start failed:', e);
            if (feedbackElementGlobal) {
                feedbackElementGlobal.innerHTML = '<span style="color: var(--danger-color);">Error: Recognition already active or failed to start. Try again.</span>';
            }
        }
    }

    // --- TTS Logic ---

    function speakWord(word) {
        if ('speechSynthesis' in window) {
            const utterance = new SpeechSynthesisUtterance(word);
            utterance.lang = 'en-US';
            utterance.volume = 1;
            utterance.rate = 1;
            utterance.pitch = 1;
            window.speechSynthesis.speak(utterance);
        } else {
            alert("Text-to-Speech is not supported in this browser.");
        }
    }

    // --- Initialization ---

    document.addEventListener('DOMContentLoaded', () => {
        initializeDots();
        showSlide(1);

        // Word Banks
        const wb4 = ["sentiment", "skepticism", "momentum", "dominant"];
        const wb8 = ["profitability", "sentiment", "anchor", "thriving", "ramifications"];
        const wb9 = ["momentum", "tempered", "mania", "scrutiny", "ecosystem"];

        createWordBank(wb4, 'wb4');
        createWordBank(wb8, 'wb8');
        createWordBank(wb9, 'wb9');

        // Pronunciation Item Shuffling
        shuffleContainerChildren('pronA-container');
        shuffleContainerChildren('pronB-container');

        // Reorder Item Shuffling
        shuffleContainerChildren('reorder-items');


        // Initialize Recognition (for all three slides)
        initRecognition('pron-status-A');
        initRecognition('pron-status-B');
        initRecognition('pron-status-C');

    });
</script>

</body>
</html>
