<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Simulador de Examen</title>
    <style>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
        font-family: Arial, sans-serif;
      }

      body {
        background-color: #f0f2f5;
        padding: 20px;
      }

      .container {
        max-width: 1200px;
        margin: 0 auto;
        background-color: white;
        border-radius: 10px;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        overflow: hidden;
      }

      header {
        background-color: #2c3e50;
        color: white;
        padding: 20px;
        text-align: center;
      }

      h1 {
        margin-bottom: 10px;
      }

      .main-content {
        display: flex;
        min-height: 500px;
      }

      .sidebar {
        width: 250px;
        background-color: #f8f9fa;
        padding: 15px;
        border-right: 1px solid #ddd;
      }

      .question-index {
        margin-bottom: 20px;
      }

      .index-title {
        margin-bottom: 10px;
        color: #2c3e50;
      }

      .index-container {
        max-height: 300px;
        overflow-y: auto;
      }

      .index-item {
        padding: 8px 12px;
        margin-bottom: 5px;
        background-color: white;
        border-radius: 5px;
        cursor: pointer;
        border-left: 4px solid #ddd;
        font-size: 14px;
        display: flex;
        justify-content: space-between;
        align-items: center;
      }

      .index-item:hover {
        background-color: #e9ecef;
      }

      .index-item.active {
        background-color: #d1ecf1;
        border-left-color: #17a2b8;
        font-weight: bold;
      }

      .status-dot {
        width: 10px;
        height: 10px;
        border-radius: 50%;
        background-color: #ccc;
      }

      .status-dot.correct {
        background-color: #28a745;
      }

      .status-dot.incorrect {
        background-color: #dc3545;
      }

      .status-dot.answered {
        background-color: #ffc107;
      }

      .stats {
        background-color: white;
        padding: 15px;
        border-radius: 5px;
        margin-top: 20px;
      }

      .exam-area {
        flex: 1;
        padding: 20px;
      }

      .question-header {
        margin-bottom: 20px;
        padding-bottom: 10px;
        border-bottom: 1px solid #ddd;
      }

      .question-text {
        font-size: 18px;
        margin-bottom: 20px;
        padding: 15px;
        background-color: #f8f9fa;
        border-radius: 5px;
      }

      .option {
        padding: 12px 15px;
        margin-bottom: 10px;
        background-color: white;
        border: 2px solid #e9ecef;
        border-radius: 5px;
        cursor: pointer;
        display: flex;
        align-items: center;
      }

      .option:hover {
        background-color: #f8f9fa;
      }

      .option.selected {
        border-color: #3498db;
        background-color: #e3f2fd;
      }

      .option.correct {
        border-color: #28a745;
        background-color: #d4edda;
      }

      .option.incorrect {
        border-color: #dc3545;
        background-color: #f8d7da;
      }

      .option-letter {
        width: 30px;
        height: 30px;
        background-color: #3498db;
        color: white;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        margin-right: 10px;
        font-weight: bold;
      }

      .buttons {
        display: flex;
        justify-content: space-between;
        margin-top: 30px;
        padding-top: 20px;
        border-top: 1px solid #ddd;
      }

      .btn {
        padding: 10px 20px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        font-size: 16px;
      }

      .btn-prev {
        background-color: #6c757d;
        color: white;
      }

      .btn-next {
        background-color: #007bff;
        color: white;
      }

      .btn-check {
        background-color: #28a745;
        color: white;
      }

      .btn:disabled {
        background-color: #ccc;
        cursor: not-allowed;
      }

      .feedback {
        margin-top: 20px;
        padding: 15px;
        border-radius: 5px;
        display: none;
      }

      .feedback.show {
        display: block;
      }

      .feedback.correct {
        background-color: #d4edda;
        border: 1px solid #c3e6cb;
        color: #155724;
      }

      .feedback.incorrect {
        background-color: #f8d7da;
        border: 1px solid #f5c6cb;
        color: #721c24;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <header>
        <h1>Simulador de Examen Tipo Test</h1>
        <p>Selecciona una respuesta y verifica si es correcta</p>
      </header>

      <div class="main-content">
        <div class="sidebar">
          <div class="question-index">
            <h3 class="index-title">Preguntas</h3>
            <div class="index-container" id="questionIndex"></div>
          </div>

          <div class="stats">
            <h3>Estadísticas</h3>
            <p>Correctas: <span id="correctCount">0</span></p>
            <p>Incorrectas: <span id="incorrectCount">0</span></p>
            <p>Sin responder: <span id="unansweredCount">5</span></p>
          </div>
        </div>

        <div class="exam-area">
          <div class="question-header">
            <h2>
              Pregunta <span id="currentQuestion">1</span> de
              <span id="totalQuestions">5</span>
            </h2>
          </div>

          <div class="question-text" id="questionText">
            An atom with 3 free electrons in its outer shell is said to be.
          </div>

          <div id="optionsContainer">
            <!-- Las opciones se generarán aquí -->
          </div>

          <div class="feedback" id="feedback">
            <!-- Retroalimentación -->
          </div>

          <div class="buttons">
            <button class="btn btn-prev" id="prevBtn" onclick="prevQuestion()">
              Anterior
            </button>
            <button class="btn btn-check" id="checkBtn" onclick="checkAnswer()">
              Verificar
            </button>
            <button class="btn btn-next" id="nextBtn" onclick="nextQuestion()">
              Siguiente
            </button>
          </div>
        </div>
      </div>
    </div>

    <script>
      // Datos de preguntas
      const questions = [
        {
          id: 1,
          category: "ISA/Atmosphere",
          text: "The ISA",
          options: [
            {
              letter: "A",
              text: "is taken from the equator.",
            },
            {
              letter: "B",
              text: "is taken from 45 degrees latitude.",
            },
            {
              letter: "C",
              text: "assumes a standard day.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "ISA stands for International Standard Atmosphere. It is a hypothetical model of atmospheric conditions (pressure, temperature, density) used as a baseline for calibrating instruments and designing aircraft. It is not a real atmosphere from a specific location.",
        },
        {
          id: 2,
          category: "ISA/Atmosphere",
          text: "At higher altitudes as altitude increases, pressure",
          options: [
            {
              letter: "A",
              text: "decreases at constant rate.",
            },
            {
              letter: "B",
              text: "decreases exponentially.",
            },
            {
              letter: "C",
              text: "increases exponentially.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Atmospheric pressure decreases exponentially with an increase in altitude. The rate of decrease is not constant; it is greatest near sea level and reduces as altitude increases.",
        },
        {
          id: 3,
          category: "ISA/Atmosphere",
          text: "When the pressure is half of that at sea level, what is the altitude?",
          options: [
            {
              letter: "A",
              text: "12,000 ft.",
            },
            {
              letter: "B",
              text: "18,000 ft.",
            },
            {
              letter: "C",
              text: "8,000 ft.",
            },
          ],
          correctAnswer: "B",
          explanation:
            'In the ISA, pressure decreases by approximately 1" Hg per 1,000 ft up to 10,000 ft, but more accurately, it halves at around 18,000 ft (specifically at 18,000 ft the pressure is about half of the sea level pressure of 29.92" Hg / 1013.2 mb).',
        },
        {
          id: 4,
          category: "ISA/Atmosphere",
          text: "If gauge pressure on a standard day at sea level is 25 PSI, the absolute pressure is",
          options: [
            {
              letter: "A",
              text: "39.7 PSI.",
            },
            {
              letter: "B",
              text: "10.3 PSI.",
            },
            {
              letter: "C",
              text: "43.8 PSI.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Absolute pressure = Gauge pressure + Atmospheric pressure. On a standard day at sea level, atmospheric pressure is 14.7 PSI. Therefore, Absolute pressure = 25 + 14.7 = 39.7 PSI.",
        },
        {
          id: 5,
          category: "ISA/Atmosphere",
          text: "Pressure decreases",
          options: [
            {
              letter: "A",
              text: "inversely proportional to temperature.",
            },
            {
              letter: "B",
              text: "proportionally with a decreases in temperature.",
            },
            {
              letter: "C",
              text: "Pressure and temperature are not related.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "According to the ideal gas law (Pressure = Density * R * Temperature), for a given volume of air, if temperature decreases, the air becomes denser and pressure can decrease. In the atmosphere, pressure and temperature generally both decrease with altitude.",
        },
        {
          id: 6,
          category: "ISA/Atmosphere",
          text: "As air gets colder, the service ceiling of an aircraft",
          options: [
            {
              letter: "A",
              text: "reduces.",
            },
            {
              letter: "B",
              text: "increases.",
            },
            {
              letter: "C",
              text: "remains the same.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Service ceiling is the altitude where the maximum rate of climb is 100 ft/min. Colder air is denser. A denser atmosphere provides more oxygen for engines (especially piston) and more lift for the wings, allowing the aircraft to climb higher.",
        },
        {
          id: 7,
          category: "ISA/Atmosphere",
          text: "What is sea level pressure?",
          options: [
            {
              letter: "A",
              text: "1012.3 mb.",
            },
            {
              letter: "B",
              text: "1013.2 mb.",
            },
            {
              letter: "C",
              text: "1032.2 mb.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The International Standard Atmosphere (ISA) defines sea level pressure as 1013.25 hectopascals (hPa), which is equivalent to 1013.25 millibars (mb) or 29.92 inches of Mercury (inHg).",
        },
        {
          id: 8,
          category: "ISA/Atmosphere",
          text: "How does IAS at the point of stall vary with height?",
          options: [
            {
              letter: "A",
              text: "It decreases.",
            },
            {
              letter: "B",
              text: "It is practically constant.",
            },
            {
              letter: "C",
              text: "It increases.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Stall occurs at a specific angle of attack. The Indicated Airspeed (IAS) at which this angle is reached is primarily a function of the aircraft's weight and configuration, not altitude. The TAS at the stall will increase with altitude, but the IAS remains practically constant.",
        },
        {
          id: 9,
          category: "ISA/Atmosphere",
          text: "What is the lapse rate with regard to temperature?",
          options: [
            {
              letter: "A",
              text: "4°C per 1000 ft.",
            },
            {
              letter: "B",
              text: "1.98°C per 1000 ft.",
            },
            {
              letter: "C",
              text: "1.98°F per 1000 ft.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The standard temperature lapse rate in the troposphere is a decrease of approximately 1.98°C (often rounded to 2°C) per 1,000 feet of altitude gain, or 6.5°C per 1,000 meters.",
        },
        {
          id: 10,
          category: "ISA/Atmosphere",
          text: "Standard sea level temperature is",
          options: [
            {
              letter: "A",
              text: "20 degrees Celsius.",
            },
            {
              letter: "B",
              text: "0 degrees Celsius.",
            },
            {
              letter: "C",
              text: "15 degrees Celsius.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The International Standard Atmosphere (ISA) defines standard sea level temperature as +15°C.",
        },
        {
          id: 11,
          category: "ISA/Atmosphere",
          text: "As altitude increases, pressure",
          options: [
            {
              letter: "A",
              text: "decreases exponentially.",
            },
            {
              letter: "B",
              text: "decreases at constant rate.",
            },
            {
              letter: "C",
              text: "increases exponentially.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Pressure decreases with altitude, and it does so exponentially, not linearly. This means the rate of decrease is higher at lower altitudes.",
        },
        {
          id: 12,
          category: "ISA/Atmosphere",
          text: "Lapse rate usually refers to",
          options: [
            {
              letter: "A",
              text: "density.",
            },
            {
              letter: "B",
              text: "pressure.",
            },
            {
              letter: "C",
              text: "temperature.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "In meteorology and aviation, the term 'lapse rate' almost exclusively refers to the rate at which temperature decreases with an increase in altitude.",
        },
        {
          id: 13,
          category: "ISA/Atmosphere",
          text: "Temperature above 36,000 feet will",
          options: [
            {
              letter: "A",
              text: "increase exponentially.",
            },
            {
              letter: "B",
              text: "decrease exponentially.",
            },
            {
              letter: "C",
              text: "remain constant.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "In the ISA, the region from approximately 36,000 ft (11 km) up to 65,000 ft (20 km) is the lower stratosphere (isothermal layer), where the temperature remains constant at -56.5°C.",
        },
        {
          id: 14,
          category: "ISA/Atmosphere",
          text: "With increasing altitude pressure decreases and",
          options: [
            {
              letter: "A",
              text: "temperature decreases at the same rate as pressure reduces.",
            },
            {
              letter: "B",
              text: "temperature decreases but at a lower rate than pressure reduces.",
            },
            {
              letter: "C",
              text: "temperature remains constant to 8000 ft.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "In the troposphere, both pressure and temperature decrease with altitude. However, pressure decreases exponentially, while temperature decreases at a constant linear lapse rate, so they do not decrease at the same rate.",
        },
        {
          id: 15,
          category: "ISA/Atmosphere",
          text: "What is the temperature in comparison to ISA conditions at 30,000ft?",
          options: [
            {
              letter: "A",
              text: "-60°C.",
            },
            {
              letter: "B",
              text: "0°C.",
            },
            {
              letter: "C",
              text: "-45°C.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Using the standard lapse rate of 1.98°C per 1000 ft from sea level (15°C), the temperature drop at 30,000 ft is approximately 30 * 1.98°C = 59.4°C. Therefore, 15°C - 59.4°C = -44.4°C, which is approximately -45°C.",
        },
        {
          id: 16,
          category: "ISA/Atmosphere",
          text: "At what altitude is the tropopause?",
          options: [
            {
              letter: "A",
              text: "36,000 ft.",
            },
            {
              letter: "B",
              text: "57,000 ft.",
            },
            {
              letter: "C",
              text: "63,000 ft.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The tropopause is the boundary between the troposphere and the stratosphere. In the ISA, its height is defined as 36,000 ft (approximately 11 km) above sea level.",
        },
        {
          id: 17,
          category: "ISA/Atmosphere",
          text: "What approximate percentage of oxygen is in the atmosphere?",
          options: [
            {
              letter: "A",
              text: "12%.",
            },
            {
              letter: "B",
              text: "21%.",
            },
            {
              letter: "C",
              text: "78%.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The Earth's atmosphere is composed of approximately 78% nitrogen and 21% oxygen, with trace amounts of other gases like argon and carbon dioxide.",
        },
        {
          id: 18,
          category: "ISA/Atmosphere",
          text: "Which has the greater density?",
          options: [
            {
              letter: "A",
              text: "Air at low altitude.",
            },
            {
              letter: "B",
              text: "Air at high altitude.",
            },
            {
              letter: "C",
              text: "It remains constant.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Air density decreases with an increase in altitude. This is because both pressure and temperature (in the troposphere) decrease, leading to the air molecules being less compressed and more spread out.",
        },
        {
          id: 19,
          category: "ISA/Atmosphere",
          text: "At what altitude does stratosphere commence approximately?",
          options: [
            {
              letter: "A",
              text: "Sea level.",
            },
            {
              letter: "B",
              text: "36,000 ft.",
            },
            {
              letter: "C",
              text: "63,000 ft.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The stratosphere is the layer of the atmosphere above the troposphere. It commences at the tropopause, which is approximately 36,000 ft in the ISA.",
        },
        {
          id: 20,
          category: "ISA/Atmosphere",
          text: "A pressure of one atmosphere is equal to",
          options: [
            {
              letter: "A",
              text: "14.7 psi.",
            },
            {
              letter: "B",
              text: "1 inch Hg.",
            },
            {
              letter: "C",
              text: "100 millibar.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "One standard atmosphere is defined as 1013.25 mb, which is equivalent to 14.7 pounds per square inch (psi) and 29.92 inches of Mercury (inHg).",
        },
        {
          id: 21,
          category: "ISA/Atmosphere",
          text: "The millibar is a unit of",
          options: [
            {
              letter: "A",
              text: "atmospheric temperature.",
            },
            {
              letter: "B",
              text: "pressure altitude.",
            },
            {
              letter: "C",
              text: "barometric pressure.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "A millibar (mb) is a metric unit of pressure, commonly used in meteorology to express barometric (atmospheric) pressure.",
        },
        {
          id: 22,
          category: "ISA/Atmosphere",
          text: "With an increase in altitude under I.S.A. conditions the temperature in the troposphere",
          options: [
            {
              letter: "A",
              text: "remains constant.",
            },
            {
              letter: "B",
              text: "decreases.",
            },
            {
              letter: "C",
              text: "increases.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "In the troposphere, under ISA conditions, temperature decreases with altitude at a standard lapse rate of approximately 1.98°C per 1000 ft.",
        },
        {
          id: 23,
          category: "ISA/Atmosphere",
          text: "A barometer indicates",
          options: [
            {
              letter: "A",
              text: "pressure.",
            },
            {
              letter: "B",
              text: "density.",
            },
            {
              letter: "C",
              text: "temperature.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "A barometer is an instrument used to measure atmospheric pressure.",
        },
        {
          id: 24,
          category: "Atmospheric Conditions",
          text: "Which condition is the actual amount of water vapour in a mixture of air and water?",
          options: [
            {
              letter: "A",
              text: "Relative humidity.",
            },
            {
              letter: "B",
              text: "Absolute humidity.",
            },
            {
              letter: "C",
              text: "Dew point.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Absolute humidity is the measure of water vapor (moisture) in the air, regardless of temperature. It is the actual mass of water vapor per unit volume of air.",
        },
        {
          id: 25,
          category: "Atmospheric Conditions",
          text: "Which will weigh the least?",
          options: [
            {
              letter: "A",
              text: "98 parts of dry air and 2 parts of water vapour.",
            },
            {
              letter: "B",
              text: "50 parts of dry air and 50 parts of water vapour.",
            },
            {
              letter: "C",
              text: "35 parts of dry air and 65 parts of water vapour.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Water vapor has a lower molecular weight (18 g/mol) than dry air (approximately 29 g/mol). Therefore, for a given volume and temperature, replacing dry air molecules with water vapor molecules makes the air lighter. The mixture with the highest proportion of water vapor will be the lightest.",
        },
        {
          id: 26,
          category: "Atmospheric Conditions",
          text: "Which is the ratio of the water vapour actually present in the atmosphere to the amount that would be present if the air were saturated at the prevailing temperature and pressure?",
          options: [
            {
              letter: "A",
              text: "Absolute humidity.",
            },
            {
              letter: "B",
              text: "Dew point.",
            },
            {
              letter: "C",
              text: "Relative humidity.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "This is the textbook definition of Relative Humidity. It is a percentage that expresses how saturated the air is with water vapor relative to its maximum capacity at a given temperature and pressure.",
        },
        {
          id: 27,
          category: "Aerodynamics",
          text: "The speed of sound in the atmosphere",
          options: [
            {
              letter: "A",
              text: "changes with a change in pressure.",
            },
            {
              letter: "B",
              text: "varies according to the frequency of the sound.",
            },
            {
              letter: "C",
              text: "changes with a change in temperature.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The speed of sound in a gas is directly proportional to the square root of the absolute temperature. It is affected by temperature, not pressure or density directly.",
        },
        {
          id: 28,
          category: "ISA/Atmosphere",
          text: "What is sea level pressure?",
          options: [
            {
              letter: "A",
              text: "1032.2 mb.",
            },
            {
              letter: "B",
              text: "1012.3 mb.",
            },
            {
              letter: "C",
              text: "1013.2 mb.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The standard sea level pressure is 1013.25 mb, which is commonly rounded to 1013.2 mb.",
        },
        {
          id: 29,
          category: "Aerodynamics",
          text: "Which statement concerning heat and/or temperature is true?",
          options: [
            {
              letter: "A",
              text: "Temperature is a measure of the kinetic energy of the molecules of any substance.",
            },
            {
              letter: "B",
              text: "Temperature is a measure of the potential energy of the molecules of any substance.",
            },
            {
              letter: "C",
              text: "There is an inverse relationship between temperature and heat.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Temperature is a measure of the average kinetic energy (energy of motion) of the molecules in a substance. Heat is the total internal energy, which includes both kinetic and potential energy.",
        },
        {
          id: 30,
          category: "Atmospheric Conditions",
          text: "What is absolute humidity?",
          options: [
            {
              letter: "A",
              text: "The amount of water vapor, usually discussed per unit volume.",
            },
            {
              letter: "B",
              text: "The temperature to which humid air must be cooled at constant pressure to become saturated.",
            },
            {
              letter: "C",
              text: "The amount of water vapor in a mixture of air and water vapor.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Absolute humidity is the mass of water vapor present in a unit volume of air, typically expressed in grams per cubic meter (g/m³).",
        },
        {
          id: 31,
          category: "Atmospheric Conditions",
          text: "The temperature to which humid air must be cooled at constant pressure to become saturated is called",
          options: [
            {
              letter: "A",
              text: "absolute humidity.",
            },
            {
              letter: "B",
              text: "dew point.",
            },
            {
              letter: "C",
              text: "relative humidity.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "This is the definition of the dew point. When air is cooled to its dew point temperature, it becomes saturated, and water vapor begins to condense.",
        },
        {
          id: 32,
          category: "ISA/Atmosphere",
          text: "Above 65,800 ft temperature",
          options: [
            {
              letter: "A",
              text: "decreases by 1.98°C up to 115,000 ft.",
            },
            {
              letter: "B",
              text: "remains constant up to 115,000 ft.",
            },
            {
              letter: "C",
              text: "increases by 0.303°C up to 115,000 ft.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "In the upper stratosphere (above 20 km / 65,600 ft), the temperature begins to increase with altitude due to the absorption of ultraviolet radiation by the ozone layer. This region is known as the upper stratosphere or mesosphere.",
        },
        {
          id: 33,
          category: "ISA/Atmosphere",
          text: "At sea level, ISA atmospheric pressure is",
          options: [
            {
              letter: "A",
              text: "14.7 kPa.",
            },
            {
              letter: "B",
              text: "10 Bar.",
            },
            {
              letter: "C",
              text: "14.7 PSI.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Standard sea level pressure is 14.7 pounds per square inch (PSI), 1013.25 mb, or 29.92 inHg. 14.7 kPa would be a much lower pressure.",
        },
        {
          id: 34,
          category: "ISA/Atmosphere",
          text: "On a very hot day with ambient temperature higher than ISA, the pressure altitude is 20,000 ft. How much will the density altitude be?",
          options: [
            {
              letter: "A",
              text: "the same.",
            },
            {
              letter: "B",
              text: "greater than 20,000ft.",
            },
            {
              letter: "C",
              text: "less than 20,000ft.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Density altitude is pressure altitude corrected for non-standard temperature. On a hot day (temperature higher than standard), the air is less dense. The density altitude will therefore be higher than the actual pressure altitude.",
        },
        {
          id: 35,
          category: "ISA/Atmosphere",
          text: "The atmospheric zone where the temperature remains fairly constant is called the",
          options: [
            {
              letter: "A",
              text: "stratosphere.",
            },
            {
              letter: "B",
              text: "ionosphere.",
            },
            {
              letter: "C",
              text: "troposphere.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "While the lower part of the stratosphere (the isothermal layer) is known for constant temperature, the 'stratosphere' as a whole is the correct answer. The question refers to the zone where temperature remains constant, which is a characteristic part of the stratosphere.",
        },
        {
          id: 36,
          category: "ISA/Atmosphere",
          text: "In the ISA the height of the tropopause is",
          options: [
            {
              letter: "A",
              text: "11,000 feet.",
            },
            {
              letter: "B",
              text: "11,000 metres.",
            },
            {
              letter: "C",
              text: "36,000 metres.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The ISA tropopause is defined at an altitude of 11 kilometers, which is approximately 36,000 feet. 11,000 meters is the correct metric equivalent.",
        },
        {
          id: 37,
          category: "ISA/Atmosphere",
          text: "In the ISA the sea level pressure is taken to be",
          options: [
            {
              letter: "A",
              text: "14 PSI.",
            },
            {
              letter: "B",
              text: "1013.2 mb.",
            },
            {
              letter: "C",
              text: "1.013 mb.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "ISA sea level pressure is defined as 1013.25 millibars (mb), which is often rounded to 1013.2 mb. 14 PSI is close but not exact, and 1.013 mb is far too low.",
        },
        {
          id: 38,
          category: "ISA/Atmosphere",
          text: "In the ISA the temperature lapse rate with altitude is taken to be:",
          options: [
            {
              letter: "A",
              text: "dependent on pressure and density changes.",
            },
            {
              letter: "B",
              text: "linear.",
            },
            {
              letter: "C",
              text: "non linear.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "In the ISA, the temperature lapse rate in the troposphere is defined as a constant, linear rate of 6.5°C per 1,000 meters (approximately 2°C per 1,000 ft).",
        },
        {
          id: 39,
          category: "ISA/Atmosphere",
          text: "Put in sequence from the ground up",
          options: [
            {
              letter: "A",
              text: "tropopause, stratosphere, troposphere.",
            },
            {
              letter: "B",
              text: "tropopause, troposphere, stratosphere.",
            },
            {
              letter: "C",
              text: "troposphere, tropopause, stratosphere.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "From the ground up, we first have the troposphere. The tropopause is the boundary layer at the top of the troposphere. Above the tropopause lies the stratosphere.",
        },
        {
          id: 40,
          category: "ISA/Atmosphere",
          text: "The International Standard Atmosphere can be described as",
          options: [
            {
              letter: "A",
              text: "the atmosphere at 45 degrees north latitude.",
            },
            {
              letter: "B",
              text: "the atmosphere at the equator with certain conditions.",
            },
            {
              letter: "C",
              text: "the atmosphere which can be used Worldwide to provide comparable performance results.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The ISA is a hypothetical model of atmospheric conditions. It is used as a common reference for calibrating instruments and comparing aircraft performance, regardless of where in the world they are tested.",
        },
        {
          id: 41,
          category: "ISA/Atmosphere",
          text: "The temperature lapse rate below the tropopause is",
          options: [
            {
              letter: "A",
              text: "1°C per 1000 ft.",
            },
            {
              letter: "B",
              text: "2°C per 1000 ft.",
            },
            {
              letter: "C",
              text: "3°C per 1000 ft.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The standard lapse rate is approximately 2°C per 1000 ft (or 1.98°C). This value is used for calculations in the troposphere.",
        },
        {
          id: 42,
          category: "ISA/Atmosphere",
          text: "Above the tropopause air pressure",
          options: [
            {
              letter: "A",
              text: "decreases at a constant rate.",
            },
            {
              letter: "B",
              text: "decreases exponentially.",
            },
            {
              letter: "C",
              text: "increases exponentially.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Pressure continues to decrease exponentially with altitude in all layers of the atmosphere, including above the tropopause. The rate of decrease changes, but the fundamental relationship remains exponential.",
        },
        {
          id: 43,
          category: "Aerodynamics",
          text: "Which of the following is correct?",
          options: [
            {
              letter: "A",
              text: "Absolute pressure + Atmospheric pressure = Gauge pressure.",
            },
            {
              letter: "B",
              text: "Absolute pressure = Gauge pressure + Atmospheric pressure.",
            },
            {
              letter: "C",
              text: "Atmospheric pressure = Absolute pressure + Gauge pressure.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Absolute pressure is measured relative to a perfect vacuum. Gauge pressure is measured relative to the ambient atmospheric pressure. Therefore, Absolute pressure = Gauge pressure + Atmospheric pressure.",
        },
        {
          id: 44,
          category: "ISA/Atmosphere",
          text: "As the altitude increases what happens of the ratio of Nitrogen to Oxygen?",
          options: [
            {
              letter: "A",
              text: "Increases.",
            },
            {
              letter: "B",
              text: "Decreases.",
            },
            {
              letter: "C",
              text: "Stays the same.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Up to very high altitudes (over 60 miles/100 km), the atmosphere is well-mixed by turbulence. The ratio of the major gases like nitrogen and oxygen remains constant, even though the overall pressure and density decrease.",
        },
        {
          id: 45,
          category: "ISA/Atmosphere",
          text: "What happens to the density of air as altitude is increased?",
          options: [
            {
              letter: "A",
              text: "Decreases.",
            },
            {
              letter: "B",
              text: "Stays the same.",
            },
            {
              letter: "C",
              text: "Increases.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Air density decreases as altitude increases. There are fewer air molecules in a given volume at higher altitudes due to the lower pressure.",
        },
        {
          id: 46,
          category: "General/Conversions",
          text: "An aircraft is travelling at a speed of 720 nautical miles per hour. To calculate speed in MPH you",
          options: [
            {
              letter: "A",
              text: "divide by 0.83.",
            },
            {
              letter: "B",
              text: "multipy by 0.83.",
            },
            {
              letter: "C",
              text: "multiply by 1.15.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "1 knot (nautical mile per hour) is equal to 1.15 statute miles per hour (MPH). To convert knots to MPH, you multiply by 1.15. 720 knots * 1.15 = 828 MPH.",
        },
        {
          id: 47,
          category: "Aerodynamics",
          text: "The CofP is the point where",
          options: [
            {
              letter: "A",
              text: "the lift can be said to act.",
            },
            {
              letter: "B",
              text: "the three axis of rotation meet.",
            },
            {
              letter: "C",
              text: "all the forces on an aircraft act.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "CofP stands for Centre of Pressure. It is the point on an airfoil where the total sum of aerodynamic pressure forces (primarily lift) is considered to act.",
        },
        {
          id: 48,
          category: "Aerodynamics",
          text: "At stall, the wingtip stagnation point",
          options: [
            {
              letter: "A",
              text: "doesn’t move.",
            },
            {
              letter: "B",
              text: "moves toward the lower surface of the wing.",
            },
            {
              letter: "C",
              text: "moves toward the upper surface of the wing.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The stagnation point is where the airflow separates to go over and under the wing. At low angles of attack, it is on the lower part of the leading edge. As angle of attack increases, it moves down and under the leading edge. At the stall, it moves significantly toward the lower surface.",
        },
        {
          id: 49,
          category: "Aerodynamics",
          text: "The rigging angle of incidence of an elevator is",
          options: [
            {
              letter: "A",
              text: "the angle between the bottom surface of the elevator and the longitudinal datum.",
            },
            {
              letter: "B",
              text: "the angle between the bottom surface of the elevator and the horizontal in the rigging position.",
            },
            {
              letter: "C",
              text: "the angle between the mean chord line and the horizontal in the rigging position.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The angle of incidence is defined by the chord line, not the bottom surface. It is the angle between the chord line of a surface (wing or tailplane) and a reference line along the aircraft, often the longitudinal axis or, in a rigging position, the horizontal.",
        },
        {
          id: 50,
          category: "Aerodynamics",
          text: "Which of the following is true?",
          options: [
            {
              letter: "A",
              text: "Lift acts at right angles to the relative airflow and weight acts vertically down.",
            },
            {
              letter: "B",
              text: "Lift acts at right angles to the wing chord line and weight acts vertically down.",
            },
            {
              letter: "C",
              text: "Lift acts at right angles to the relative air flow and weight acts at right angles to the aircraft centre line.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "By definition, lift is the component of the total aerodynamic force that is perpendicular to the relative airflow. Weight always acts vertically downward towards the center of the Earth.",
        },
        {
          id: 51,
          category: "Aerodynamics",
          text: "What happens to air flowing at the speed of sound when it enters a converging duct?",
          options: [
            {
              letter: "A",
              text: "Velocity increases, pressure and density decreases.",
            },
            {
              letter: "B",
              text: "Velocity, pressure and density increase.",
            },
            {
              letter: "C",
              text: "Velocity decreases, pressure and density increase.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "For supersonic flow (M >= 1), a converging duct acts as a diffuser. The velocity decreases, and pressure and density increase.",
        },
        {
          id: 52,
          category: "Aerodynamics",
          text: "As the angle of attack of an airfoil increases the centre of pressure",
          options: [
            {
              letter: "A",
              text: "remains stationary.",
            },
            {
              letter: "B",
              text: "moves aft.",
            },
            {
              letter: "C",
              text: "moves forward.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "On a conventional cambered airfoil, as angle of attack increases, the center of pressure moves forward towards the leading edge. At very low angles, it is further aft.",
        },
        {
          id: 53,
          category: "Aerodynamics",
          text: "Vapour trails from the wingtips of an aircraft in flight are caused by",
          options: [
            {
              letter: "A",
              text: "low pressure above the wing and high pressure below the wing causing vortices.",
            },
            {
              letter: "B",
              text: "low pressure above the wing and high pressure below the wing causing a temperature rise.",
            },
            {
              letter: "C",
              text: "high pressure above the wing and low pressure below the wing causing vortices.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The pressure difference between the upper and lower surfaces causes air to spill around the wingtip, creating a vortex. In the core of the vortex, pressure and temperature drop, causing water vapor to condense and become visible as a vapor trail.",
        },
        {
          id: 54,
          category: "Aerodynamics",
          text: "The chord line of a wing is a line that runs from",
          options: [
            {
              letter: "A",
              text: "the centre of the leading edge of the wing to the trailing edge.",
            },
            {
              letter: "B",
              text: "half way between the upper and lower surface of the wing.",
            },
            {
              letter: "C",
              text: "one wing tip to the other wing tip.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The chord line is a straight line connecting the leading edge and the trailing edge of an airfoil.",
        },
        {
          id: 55,
          category: "Aerodynamics",
          text: "The angle of incidence of a wing is an angle formed by lines",
          options: [
            {
              letter: "A",
              text: "parallel to the chord line and longitudinal axis.",
            },
            {
              letter: "B",
              text: "parallel to the chord line and the vertical axis.",
            },
            {
              letter: "C",
              text: "parallel to the chord line and the lateral axis.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Angle of incidence is the angle between the chord line of the wing and the longitudinal axis of the aircraft. It is a fixed angle built into the aircraft design.",
        },
        {
          id: 56,
          category: "Aerodynamics",
          text: "The centre of pressure of an aerofoil is located",
          options: [
            {
              letter: "A",
              text: "30 - 40% of the chord line forward of the leading edge.",
            },
            {
              letter: "B",
              text: "50% of the chord line back from the leading edge.",
            },
            {
              letter: "C",
              text: "30 - 40% of the chord line back from the leading edge.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "For subsonic airfoils at cruising angles of attack, the center of pressure is typically located between 30% and 40% of the chord length aft of the leading edge.",
        },
        {
          id: 57,
          category: "Aerodynamics",
          text: "Compressibility effect is",
          options: [
            {
              letter: "A",
              text: "drag associated with the form of an aircraft.",
            },
            {
              letter: "B",
              text: "the increase in total drag of an aerofoil in transonic flight due to the formation of shock waves.",
            },
            {
              letter: "C",
              text: "drag associated with the friction of the air over the surface of the aircraft.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Compressibility effects, particularly drag rise, occur in transonic flight when local airflow over the wing reaches supersonic speeds, forming shock waves. This leads to a significant increase in drag, known as wave drag.",
        },
        {
          id: 58,
          category: "Aerodynamics",
          text: "A high aspect ratio wing will give",
          options: [
            {
              letter: "A",
              text: "high profile and low induced drag.",
            },
            {
              letter: "B",
              text: "low profile and high induced drag.",
            },
            {
              letter: "C",
              text: "low profile and low induced drag.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "A high aspect ratio wing (long and slender) produces less induced drag for the same amount of lift. Profile drag is more related to surface area and airfoil shape, not aspect ratio directly, so a well-designed high AR wing aims for low total drag.",
        },
        {
          id: 59,
          category: "Aerodynamics",
          text: "Aerofoil efficiency is defined by",
          options: [
            {
              letter: "A",
              text: "lift over drag.",
            },
            {
              letter: "B",
              text: "lift over weight.",
            },
            {
              letter: "C",
              text: "drag over lift.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The Lift-to-Drag ratio (L/D) is a measure of the efficiency of an airfoil or aircraft. A higher L/D ratio means it is more efficient, producing more lift for a given amount of drag.",
        },
        {
          id: 60,
          category: "Aerodynamics",
          text: "The relationship between induced drag and airspeed is, induced drag is",
          options: [
            {
              letter: "A",
              text: "directly proportional to the square of the speed.",
            },
            {
              letter: "B",
              text: "directly proportional to speed.",
            },
            {
              letter: "C",
              text: "inversely proportional to the square of the speed.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Induced drag is high at low speeds and low at high speeds. It is inversely proportional to the square of the airspeed (Induced Drag ∝ 1/V²).",
        },
        {
          id: 61,
          category: "Aerodynamics",
          text: "What is the definition of Angle of Incidence?",
          options: [
            {
              letter: "A",
              text: "The angle the underside of the mainplane or tailplane makes with the horizontal.",
            },
            {
              letter: "B",
              text: "The angle the underside of the mainplane or tailplane makes with the longitudinal datum line.",
            },
            {
              letter: "C",
              text: "The angle the chord of the mainplane or tailplane makes with the horizontal.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Angle of incidence is defined by the chord line, not the underside. It is the angle between the chord line of the wing/tailplane and a reference line along the aircraft, usually the longitudinal axis, which is horizontal in level flight.",
        },
        {
          id: 62,
          category: "Aerodynamics",
          text: "What is Boundary Layer?",
          options: [
            {
              letter: "A",
              text: "Separated layer of air forming a boundary at the leading edge.",
            },
            {
              letter: "B",
              text: "Sluggish low energy air that sticks to the wing surface and gradually gets faster until it joins the free stream flow of air.",
            },
            {
              letter: "C",
              text: "Turbulent air moving from the leading edge to trailing edge.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The boundary layer is the thin layer of air close to the surface of the wing where viscous forces are significant. The air velocity within this layer ranges from zero at the surface (due to friction) to the free-stream velocity at its edge.",
        },
        {
          id: 63,
          category: "Aircraft Design",
          text: "What is the collective term for the fin and rudder and other surfaces aft of the centre of gravity that helps directional stability?",
          options: [
            {
              letter: "A",
              text: "Empennage.",
            },
            {
              letter: "B",
              text: "Fuselage surfaces.",
            },
            {
              letter: "C",
              text: "Effective keel surface.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The empennage, also known as the tail section, includes the vertical stabilizer (fin), rudder, horizontal stabilizer, and elevators. The fin and rudder specifically provide directional stability.",
        },
        {
          id: 64,
          category: "Aerodynamics",
          text: "A decrease in incidence toward the wing tip may be provided to",
          options: [
            {
              letter: "A",
              text: "prevent adverse yaw in a turn.",
            },
            {
              letter: "B",
              text: "retain lateral control effectiveness at high angles of attack.",
            },
            {
              letter: "C",
              text: "prevent span wise flow in maneuvers.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "This is called 'washout'. By reducing the angle of incidence at the tip, the wing root will stall before the tip. This ensures that the ailerons, located near the tips, remain effective for lateral control as the stall progresses.",
        },
        {
          id: 65,
          category: "Aerodynamics",
          text: "Low wing loading",
          options: [
            {
              letter: "A",
              text: "increases stalling speed, landing speed and landing run.",
            },
            {
              letter: "B",
              text: "increases lift, stalling speed and maneuverability.",
            },
            {
              letter: "C",
              text: "decreases stalling speed, landing speed and landing run.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Wing loading is Weight / Wing Area. Lower wing loading means the wing has to generate less lift per unit area to support the aircraft's weight. This results in a lower stalling speed, which in turn leads to lower landing and takeoff speeds and shorter ground runs.",
        },
        {
          id: 66,
          category: "Aerodynamics",
          text: "As a general rule, if the aerodynamic angle of incidence (angle of attack) of an aerofoil is slightly increased, the centre of pressure will",
          options: [
            {
              letter: "A",
              text: "move towards the tip.",
            },
            {
              letter: "B",
              text: "move forward towards the leading edge.",
            },
            {
              letter: "C",
              text: "never move.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "For a cambered airfoil, an increase in angle of attack causes the center of pressure to move forward.",
        },
        {
          id: 67,
          category: "Aerodynamics",
          text: "When does the angle of incidence change?",
          options: [
            {
              letter: "A",
              text: "It never changes.",
            },
            {
              letter: "B",
              text: "When the aircraft attitude changes.",
            },
            {
              letter: "C",
              text: "When the aircraft is ascending or descending.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The angle of incidence is a fixed geometric angle built into the aircraft. It is the angle between the wing chord line and the longitudinal axis of the aircraft. It does not change in flight.",
        },
        {
          id: 68,
          category: "Aerodynamics",
          text: "As the angle of attack decreases, what happens to the centre of pressure?",
          options: [
            {
              letter: "A",
              text: "It moves rearwards.",
            },
            {
              letter: "B",
              text: "Centre of pressure is not affected by angle of attack decrease.",
            },
            {
              letter: "C",
              text: "It moves forward.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "As angle of attack decreases, the center of pressure moves rearward along the chord line. (Opposite to the effect of increasing AoA).",
        },
        {
          id: 69,
          category: "Aerodynamics",
          text: "A decrease in pressure over the upper surface of a wing or aerofoil is responsible for",
          options: [
            {
              letter: "A",
              text: "approximately 2/3 (two thirds) of the lift obtained.",
            },
            {
              letter: "B",
              text: "approximately 1/2 (one half) of the lift obtained.",
            },
            {
              letter: "C",
              text: "approximately 1/3 (one third) of the lift obtained.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Lift is generated by a pressure difference. Approximately two-thirds of the lift comes from the lower pressure (suction) on the upper surface, and about one-third comes from the higher pressure on the lower surface.",
        },
        {
          id: 70,
          category: "Aerodynamics",
          text: "Which of the following types of drag increases as the aircraft gains altitude?",
          options: [
            {
              letter: "A",
              text: "Interference drag.",
            },
            {
              letter: "B",
              text: "Parasite drag.",
            },
            {
              letter: "C",
              text: "Induced drag.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "For a given IAS, as altitude increases (density decreases), the True Airspeed (TAS) increases. Since induced drag is a function of lift and dynamic pressure, and dynamic pressure is ½ρV², if IAS is constant, ρV² is constant, so induced drag is primarily a function of lift and remains relatively constant for a given IAS. However, for a given TAS, induced drag decreases with altitude. The question is ambiguous, but in many contexts, for a given weight and IAS, induced drag remains similar. Parasite and interference drag are forms of parasite drag and increase with TAS².",
        },
        {
          id: 71,
          category: "Aerodynamics",
          text: "The layer of air over the surface of an aerofoil which is slower moving, in relation to the rest of the airflow, is known as",
          options: [
            {
              letter: "A",
              text: "none of the above.",
            },
            {
              letter: "B",
              text: "camber layer.",
            },
            {
              letter: "C",
              text: "boundary layer.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "This is the definition of the boundary layer. It's the region where viscous forces slow the air down relative to the free stream.",
        },
        {
          id: 72,
          category: "Aerodynamics",
          text: "What is a controlling factor of turbulence and skin friction?",
          options: [
            {
              letter: "A",
              text: "Countersunk rivets used on skin exterior.",
            },
            {
              letter: "B",
              text: "Aspect ratio.",
            },
            {
              letter: "C",
              text: "Fineness ratio.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Surface finish is a major controlling factor for skin friction. Countersunk rivets create a smoother surface compared to protruding head rivets, thus reducing skin friction and helping to delay the transition from laminar to turbulent flow.",
        },
        {
          id: 73,
          category: "Stability",
          text: "If the C of G is aft of the Centre of Pressure",
          options: [
            {
              letter: "A",
              text: "when the aircraft yaws the aerodynamic forces acting forward of the Centre of Pressure.",
            },
            {
              letter: "B",
              text: "changes in lift produce a pitching moment which acts to increase the change in lift.",
            },
            {
              letter: "C",
              text: "when the aircraft sideslips, the C of G causes the nose to turn into the sideslip thus applying a restoring moment.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "If the CG is aft of the CP, the lift force (acting at CP) creates a moment around the CG. If the aircraft pitches nose-up, lift increases, and because the CP is ahead of the CG, this increase in lift creates a further nose-up moment, which is unstable.",
        },
        {
          id: 74,
          category: "Aerodynamics",
          text: "The upper part of the wing in comparison to the lower",
          options: [
            {
              letter: "A",
              text: "develops less lift.",
            },
            {
              letter: "B",
              text: "develops the same lift.",
            },
            {
              letter: "C",
              text: "develops more lift.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The upper surface, due to its curvature, causes the airflow to accelerate, creating a low-pressure area. This pressure differential (low above, high below) is what generates lift, with the upper surface contributing approximately two-thirds of the total lift.",
        },
        {
          id: 75,
          category: "Stability",
          text: "What effect would a forward CG have on an aircraft on landing?",
          options: [
            {
              letter: "A",
              text: "Increase stalling speed.",
            },
            {
              letter: "B",
              text: "Reduce stalling speed.",
            },
            {
              letter: "C",
              text: "No effect on landing.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "A forward CG means the tailplane must produce more downforce to balance the aircraft. This downforce adds to the total lift that the wing must generate. To generate this extra lift, the wing must fly at a higher angle of attack and higher speed, thus increasing the stalling speed.",
        },
        {
          id: 76,
          category: "Instruments",
          text: "QNH refers to",
          options: [
            {
              letter: "A",
              text: "quite near horizon.",
            },
            {
              letter: "B",
              text: "setting the altimeter to zero.",
            },
            {
              letter: "C",
              text: "setting the mean sea level atmospheric pressure so an altimeter reads the aerodrome altitude above mean sea level.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "QNH is a Q-code meaning 'Atmospheric pressure at mean sea level'. When set on an altimeter, it allows the instrument to read altitude above mean sea level (AMSL). On the ground at the airport, it will read the airport's elevation.",
        },
        {
          id: 77,
          category: "Instruments",
          text: "QNE refers to",
          options: [
            {
              letter: "A",
              text: "setting the mean sea level atmospheric pressure in accordance with ICAO standard atmosphere i.e. 1013 millibars.",
            },
            {
              letter: "B",
              text: "Setting an altimeter to read aerodrome altitude above sea level.",
            },
            {
              letter: "C",
              text: "quite new equipment.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "QNE is the Q-code for the standard altimeter setting of 1013.25 hPa / mb. It is used for flying in the upper airspace to ensure all aircraft have the same reference, thus maintaining vertical separation.",
        },
        {
          id: 78,
          category: "Aircraft Design",
          text: "An aspect ratio of 8 : 1 would mean",
          options: [
            {
              letter: "A",
              text: "span 64, mean chord 8.",
            },
            {
              letter: "B",
              text: "mean chord 64, span 8.",
            },
            {
              letter: "C",
              text: "span squared 64, chord 8.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Aspect Ratio (AR) is calculated as Span² / Area. If AR = 8, and we take a simple example where the wing is rectangular, Area = Span * Chord. So AR = Span² / (Span * Chord) = Span / Chord. Therefore, a ratio of 8:1 means Span = 8 and Chord = 1. If Span = 64 and Chord = 8, then AR = 64/8 = 8. So option A works with that scaling.",
        },
        {
          id: 79,
          category: "Instruments",
          text: "QFE is",
          options: [
            {
              letter: "A",
              text: "airfield pressure.",
            },
            {
              letter: "B",
              text: "difference between sea level and airfield pressure.",
            },
            {
              letter: "C",
              text: "sea level pressure.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "QFE is the Q-code for atmospheric pressure at a specific airfield elevation. When set on an altimeter, it reads height above that specific airfield reference point. On the ground, it reads zero.",
        },
        {
          id: 80,
          category: "Aerodynamics",
          text: "For any given speed, a decrease in aircraft weight, the induced drag will",
          options: [
            {
              letter: "A",
              text: "decrease.",
            },
            {
              letter: "B",
              text: "remain the same.",
            },
            {
              letter: "C",
              text: "increase.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Induced drag is a byproduct of generating lift. If the aircraft is lighter, it requires less lift to maintain level flight at a given speed. Generating less lift results in lower induced drag.",
        },
        {
          id: 81,
          category: "Aerodynamics",
          text: "The amount of lift generated by a wing is",
          options: [
            {
              letter: "A",
              text: "greatest at the tip.",
            },
            {
              letter: "B",
              text: "constant along the span.",
            },
            {
              letter: "C",
              text: "greatest at the root.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "On a typical untwisted wing, the lift per unit span is greatest at the wing root and decreases towards the tips. This is due to the spanwise pressure gradients and the influence of wingtip vortices.",
        },
        {
          id: 82,
          category: "Aerodynamics",
          text: "Induced Drag is",
          options: [
            {
              letter: "A",
              text: "greatest towards the tip and downwash decreases from tip to root.",
            },
            {
              letter: "B",
              text: "greatest towards the wing tip and downwash is greatest towards the root.",
            },
            {
              letter: "C",
              text: "greatest towards the wing root and downwash is greatest at the tip.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Induced drag is a direct result of the downwash angle. The downwash angle is greatest near the wingtips due to the strength of the vortices there. Therefore, induced drag is highest at the tips and decreases towards the root.",
        },
        {
          id: 83,
          category: "Aerodynamics",
          text: "Induced Drag is",
          options: [
            {
              letter: "A",
              text: "never equal to profile drag.",
            },
            {
              letter: "B",
              text: "equal to profile drag at Vmd.",
            },
            {
              letter: "C",
              text: "equal to profile drag at stalling angle.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Vmd is the speed for minimum drag. Total drag = Profile Drag (Parasite + Form + Skin Friction) + Induced Drag. At the speed for minimum drag (Vmd), profile drag equals induced drag.",
        },
        {
          id: 84,
          category: "Aerodynamics",
          text: "With an increase in aircraft weight",
          options: [
            {
              letter: "A",
              text: "Vmd will be at a higher speed.",
            },
            {
              letter: "B",
              text: "Vmd will be at the same speed.",
            },
            {
              letter: "C",
              text: "Vmd will be at a lower speed.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Vmd (the speed for minimum drag) is not constant; it increases with aircraft weight. For a heavier aircraft, more lift is required, which increases induced drag. To bring total drag back to a minimum (where induced drag equals profile drag), the speed must increase.",
        },
        {
          id: 85,
          category: "Aerodynamics",
          text: "For a given IAS an increase in altitude will result in",
          options: [
            {
              letter: "A",
              text: "an increase in induced drag.",
            },
            {
              letter: "B",
              text: "no change in the value of induced drag.",
            },
            {
              letter: "C",
              text: "an increase in profile drag.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "For a given IAS and weight, the lift required is constant. Induced drag is primarily a function of the lift coefficient. Since IAS is constant, the dynamic pressure (½ρV²) is constant, meaning the lift coefficient (CL) is constant. Therefore, induced drag remains the same.",
        },
        {
          id: 86,
          category: "Aerodynamics",
          text: "Stall inducers may be fitted to a wing",
          options: [
            {
              letter: "A",
              text: "at the root to cause the root to stall first.",
            },
            {
              letter: "B",
              text: "at the tip to cause the root to stall first.",
            },
            {
              letter: "C",
              text: "at the root to cause the tip to stall first.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Stall inducers, or stall strips, are small wedges attached to the leading edge of the wing root. They intentionally disrupt the airflow at the root at high angles of attack, ensuring that the root stalls before the tip. This maintains aileron effectiveness.",
        },
        {
          id: 87,
          category: "Aerodynamics",
          text: "The optimum angle of attack of an aerofoil is the angle at which",
          options: [
            {
              letter: "A",
              text: "the aerofoil produces maximum lift.",
            },
            {
              letter: "B",
              text: "the aerofoil produces zero lift.",
            },
            {
              letter: "C",
              text: "the highest lift/drag ratio is produced.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The 'optimum' angle of attack is the most efficient one, which is where the Lift-to-Drag ratio is at its maximum. This is also known as the angle for best glide or best L/D.",
        },
        {
          id: 88,
          category: "Aerodynamics",
          text: "Minimum total drag of an aircraft occurs",
          options: [
            {
              letter: "A",
              text: "when induced drag is least.",
            },
            {
              letter: "B",
              text: "at the stalling speed.",
            },
            {
              letter: "C",
              text: "when profile drag equals induced drag.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Total drag is the sum of profile (parasite) drag and induced drag. Minimum total drag occurs at the speed where these two components are equal. This is known as Vmd.",
        },
        {
          id: 89,
          category: "Aerodynamics",
          text: "If the weight of an aircraft is increased, the induced drag at a given speed",
          options: [
            {
              letter: "A",
              text: "will increase.",
            },
            {
              letter: "B",
              text: "will decrease.",
            },
            {
              letter: "C",
              text: "will remain the same.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "To maintain level flight at a given speed with a higher weight, the aircraft must generate more lift. This requires a higher angle of attack, which results in a greater induced drag.",
        },
        {
          id: 90,
          category: "Aerodynamics",
          text: "The transition point on a wing is the point where",
          options: [
            {
              letter: "A",
              text: "the boundary layer flow changes from laminar to turbulent.",
            },
            {
              letter: "B",
              text: "the flow divides to pass above and below the wing.",
            },
            {
              letter: "C",
              text: "the flow separates from the wing surface.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The transition point is the location on the wing surface where the smooth, layered laminar flow breaks down into chaotic, mixing turbulent flow.",
        },
        {
          id: 91,
          category: "Aerodynamics",
          text: "A laminar boundary layer will produce",
          options: [
            {
              letter: "A",
              text: "more skin friction drag than a turbulent one.",
            },
            {
              letter: "B",
              text: "the same skin friction drag as a turbulent one.",
            },
            {
              letter: "C",
              text: "less skin friction drag than a turbulent one.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Laminar flow is smooth and orderly, with lower velocity gradients at the surface, resulting in lower skin friction compared to turbulent flow, which has higher energy mixing and greater surface friction.",
        },
        {
          id: 92,
          category: "Aerodynamics",
          text: "The boundary layer is",
          options: [
            {
              letter: "A",
              text: "thickest at the leading edge.",
            },
            {
              letter: "B",
              text: "thickest at the trailing edge.",
            },
            {
              letter: "C",
              text: "constant thickness from leading to trailing edges.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The boundary layer starts at zero thickness at the leading edge and grows thicker as it moves along the surface towards the trailing edge.",
        },
        {
          id: 93,
          category: "Propulsion",
          text: "The amount of thrust produced by a jet engine or a propeller can be calculated using",
          options: [
            {
              letter: "A",
              text: "Newton’s 3rd law.",
            },
            {
              letter: "B",
              text: "Newton’s 2nd law.",
            },
            {
              letter: "C",
              text: "Newton’s 1st law.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "While thrust is often associated with Newton's 3rd Law (action-reaction), the *amount* of thrust is calculated using Newton's 2nd Law: Force = mass flow rate * change in velocity (F = ṁ * ΔV).",
        },
        {
          id: 94,
          category: "Propulsion",
          text: "An engine which produces an efflux of high speed will be",
          options: [
            {
              letter: "A",
              text: "less efficient.",
            },
            {
              letter: "B",
              text: "more efficient.",
            },
            {
              letter: "C",
              text: "speed of efflux has no affect on the engine efficiency.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Propulsive efficiency is highest when the exhaust velocity is close to the aircraft's forward speed. A high-speed efflux (like a pure turbojet) is less efficient at subsonic speeds because it wastes a lot of kinetic energy. A high bypass turbofan, with a larger, slower efflux, is more efficient.",
        },
        {
          id: 95,
          category: "Aircraft Design",
          text: "Wing loading is calculated by weight",
          options: [
            {
              letter: "A",
              text: "divided by lift.",
            },
            {
              letter: "B",
              text: "divided by gross wing area.",
            },
            {
              letter: "C",
              text: "multiplied by gross wing area.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Wing loading is a measure of how much weight each unit area of the wing must support. It is calculated by dividing the aircraft's weight by its gross wing area.",
        },
        {
          id: 96,
          category: "Aerodynamics",
          text: "Induced drag is",
          options: [
            {
              letter: "A",
              text: "nothing to do with speed.",
            },
            {
              letter: "B",
              text: "proportional to speed.",
            },
            {
              letter: "C",
              text: "inversely proportional to the square of speed.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Induced drag is high at low speeds and low at high speeds. Its relationship with speed is inverse and squared (Induced Drag ∝ 1/V²).",
        },
        {
          id: 97,
          category: "Aerodynamics",
          text: "As the angle of attack increases the stagnation point",
          options: [
            {
              letter: "A",
              text: "moves towards the upper surface.",
            },
            {
              letter: "B",
              text: "does not move.",
            },
            {
              letter: "C",
              text: "moves towards the lower surface.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The stagnation point is where the airflow comes to rest. As angle of attack increases, this point moves down and aft along the lower surface of the leading edge.",
        },
        {
          id: 98,
          category: "Aerodynamics",
          text: "The term pitch-up is due to",
          options: [
            {
              letter: "A",
              text: "compressibility effect.",
            },
            {
              letter: "B",
              text: "ground effect.",
            },
            {
              letter: "C",
              text: "longitudinal instability.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "'Pitch-up' is a specific compressibility effect that can occur on swept-wing aircraft at transonic speeds. Shock waves can cause the outboard sections of the wing to stall first, shifting the center of pressure forward and causing a sudden, powerful nose-up pitch.",
        },
        {
          id: 99,
          category: "Aerodynamics",
          text: "In a steady climb at a steady IAS, the TAS is",
          options: [
            {
              letter: "A",
              text: "more than IAS.",
            },
            {
              letter: "B",
              text: "the same.",
            },
            {
              letter: "C",
              text: "less than IAS.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "As altitude increases, air density decreases. To maintain the same IAS (dynamic pressure), the aircraft must move faster through the air. Therefore, TAS is always greater than IAS in a climb, assuming a standard lapse rate.",
        },
        {
          id: 100,
          category: "Aerodynamics",
          text: "An untapered straight wing will",
          options: [
            {
              letter: "A",
              text: "have no yaw effect in banking.",
            },
            {
              letter: "B",
              text: "stall at the root first.",
            },
            {
              letter: "C",
              text: "have no change in induced drag in the bank.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "An untapered (rectangular) wing typically has a higher thickness/chord ratio and experiences higher induced angles of attack at the root. This causes the root to stall before the tip, which is a desirable stall characteristic.",
        },
        {
          id: 101,
          category: "Aerodynamics",
          text: "With the ailerons away from the neutral, induced drag is",
          options: [
            {
              letter: "A",
              text: "higher on the lower wing plus profile drag increases.",
            },
            {
              letter: "B",
              text: "unchanged but profile drag is higher.",
            },
            {
              letter: "C",
              text: "higher on the upper wing plus profile drag increases.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "When ailerons are deflected, the downgoing aileron (on the wing that will rise) increases camber and lift, increasing induced drag on that wing. The upgoing aileron decreases lift. This difference in drag (adverse yaw) is an induced drag effect.",
        },
        {
          id: 102,
          category: "Aerodynamics",
          text: "All the lift can be said to act through the",
          options: [
            {
              letter: "A",
              text: "centre of pressure.",
            },
            {
              letter: "B",
              text: "centre of gravity.",
            },
            {
              letter: "C",
              text: "normal axis.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The Centre of Pressure (CP) is the conceptual point where the total sum of the lift forces is considered to be concentrated.",
        },
        {
          id: 103,
          category: "Propulsion",
          text: "The concept of thrust is explained by",
          options: [
            {
              letter: "A",
              text: "Bernoulli’s theorem.",
            },
            {
              letter: "B",
              text: "Newton’s 3rd law.",
            },
            {
              letter: "C",
              text: "Newton’s 1st law.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Thrust is the reaction force that propels an aircraft forward. A propeller accelerates air backwards, and a jet engine accelerates exhaust gases backwards. According to Newton's 3rd Law, for every action (accelerating air/gases backwards), there is an equal and opposite reaction (thrust forwards).",
        },
        {
          id: 104,
          category: "Aerodynamics",
          text: "The camber of an aerofoil section is",
          options: [
            {
              letter: "A",
              text: "the angle which the aerofoil makes with the relative airflow.",
            },
            {
              letter: "B",
              text: "the curvature of the median line of the aerofoil.",
            },
            {
              letter: "C",
              text: "the angle of incidence towards the tip of a wing.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Camber is the asymmetry or curvature of an airfoil. It is defined by the curve of the median line, which is halfway between the upper and lower surfaces.",
        },
        {
          id: 105,
          category: "Aerodynamics",
          text: "As air flows over the upper cambered surface of an aerofoil, what happens to velocity and pressure?",
          options: [
            {
              letter: "A",
              text: "Velocity increases, pressure increases.",
            },
            {
              letter: "B",
              text: "Velocity increases, pressure decreases.",
            },
            {
              letter: "C",
              text: "Velocity decreases, pressure decreases.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Due to the curvature, the air must travel a longer distance over the top surface. To maintain a constant mass flow, it speeds up. According to Bernoulli's principle, an increase in velocity results in a decrease in static pressure.",
        },
        {
          id: 106,
          category: "Aerodynamics",
          text: "What is the force that tends to pull an aircraft down towards the earth?",
          options: [
            {
              letter: "A",
              text: "Thrust.",
            },
            {
              letter: "B",
              text: "Weight.",
            },
            {
              letter: "C",
              text: "Drag.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Weight is the force of gravity acting on the aircraft's mass, pulling it towards the center of the Earth.",
        },
        {
          id: 107,
          category: "Aerodynamics",
          text: "The angle at which the chord line of the aerofoil is presented to the airflow is known as",
          options: [
            {
              letter: "A",
              text: "angle of attack.",
            },
            {
              letter: "B",
              text: "resultant.",
            },
            {
              letter: "C",
              text: "angle of incidence.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Angle of Attack (AoA) is defined as the angle between the chord line of the airfoil and the direction of the oncoming airflow (relative wind).",
        },
        {
          id: 108,
          category: "Aerodynamics",
          text: "The imaginary straight line which passes through an aerofoil section from leading edge to trailing edge is called",
          options: [
            {
              letter: "A",
              text: "the chord line.",
            },
            {
              letter: "B",
              text: "the direction of relative airflow.",
            },
            {
              letter: "C",
              text: "centre of pressure.",
            },
          ],
          correctAnswer: "A",
          explanation: "This is the definition of the chord line.",
        },
        {
          id: 109,
          category: "Aerodynamics",
          text: "What is the angle between the chord line of the wing, and the longitudinal axis of the aircraft, known as?",
          options: [
            {
              letter: "A",
              text: "Angle of dihedral.",
            },
            {
              letter: "B",
              text: "Angle of attack.",
            },
            {
              letter: "C",
              text: "Angle of incidence.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "This is the definition of the angle of incidence. It is a fixed angle determined by the aircraft's design.",
        },
        {
          id: 110,
          category: "Aerodynamics",
          text: "Wing tip vortices create a type of drag known as",
          options: [
            {
              letter: "A",
              text: "form drag.",
            },
            {
              letter: "B",
              text: "profile drag.",
            },
            {
              letter: "C",
              text: "induced drag.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The vortices generated at the wingtips are the physical manifestation of induced drag. They represent energy being lost from the wing to create lift.",
        },
        {
          id: 111,
          category: "Aerodynamics",
          text: "As the angle of attack is increased (up to the stall point), which of the following is correct?",
          options: [
            {
              letter: "A",
              text: "Both are correct.",
            },
            {
              letter: "B",
              text: "Pressure difference between top and bottom of the wing increases.",
            },
            {
              letter: "C",
              text: "Lift increases.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "As AoA increases up to the critical angle, the pressure difference between the upper and lower surfaces increases, which results in an increase in lift. Both statements are correct.",
        },
        {
          id: 112,
          category: "Aerodynamics",
          text: "What type of drag, depends on the smoothness of the body, and surface area over which the air flows?",
          options: [
            {
              letter: "A",
              text: "Form drag.",
            },
            {
              letter: "B",
              text: "Parasite drag.",
            },
            {
              letter: "C",
              text: "Skin friction drag.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Skin friction drag is caused by the viscosity of the air as it flows over the aircraft's surface. It depends directly on the smoothness of the surface and the total surface area exposed to the airflow.",
        },
        {
          id: 113,
          category: "Aerodynamics",
          text: "When airflow velocity over an upper cambered surface of an aerofoil decreases, what takes place?",
          options: [
            {
              letter: "A",
              text: "Pressure decreases, lift increases.",
            },
            {
              letter: "B",
              text: "Pressure increases, lift decreases.",
            },
            {
              letter: "C",
              text: "Pressure increases, lift increases.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "A decrease in velocity on the upper surface (which would happen at very high angles of attack approaching the stall) leads to an increase in pressure on that surface (recovery towards ambient). This reduces the pressure differential between upper and lower surfaces, thereby decreasing lift.",
        },
        {
          id: 114,
          category: "Aerodynamics",
          text: "When an aircraft stalls",
          options: [
            {
              letter: "A",
              text: "lift increases and drag decreases.",
            },
            {
              letter: "B",
              text: "lift and drag increase.",
            },
            {
              letter: "C",
              text: "lift decreases and drag increases.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "At the stall, the airflow separates from the upper surface. This causes a dramatic loss of lift and a significant increase in drag due to the turbulent wake behind the wing.",
        },
        {
          id: 115,
          category: "Aircraft Design",
          text: "An aircraft wing with an aspect ration of 6:1 is proportional so that",
          options: [
            {
              letter: "A",
              text: "the wing area is six times the span.",
            },
            {
              letter: "B",
              text: "the mean chord is six times the thickness.",
            },
            {
              letter: "C",
              text: "the wing span is six times the mean chord.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Aspect Ratio (AR) = Span / Mean Chord. Therefore, an AR of 6:1 means the span is six times the mean chord.",
        },
        {
          id: 116,
          category: "Aircraft Design",
          text: "Upward and outward inclination of a mainplane is termed",
          options: [
            {
              letter: "A",
              text: "dihedral.",
            },
            {
              letter: "B",
              text: "sweep.",
            },
            {
              letter: "C",
              text: "stagger.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Dihedral is the angle at which the wings are inclined upward from the root to the tip. It is a design feature to enhance lateral stability.",
        },
        {
          id: 117,
          category: "Aerodynamics",
          text: "Which of the following forces act on an aircraft in level flight?",
          options: [
            {
              letter: "A",
              text: "Lift, drag, thrust.",
            },
            {
              letter: "B",
              text: "Lift, thrust, and weight.",
            },
            {
              letter: "C",
              text: "Lift, thrust, weight, and drag.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The four forces acting on an aircraft in flight are Lift, Weight (Gravity), Thrust, and Drag.",
        },
        {
          id: 118,
          category: "Instruments",
          text: "With reference to altimeters, QFE is",
          options: [
            {
              letter: "A",
              text: "the manufacturers registered name.",
            },
            {
              letter: "B",
              text: "quite fine equipment.",
            },
            {
              letter: "C",
              text: "setting aerodrome atmospheric pressure so that an altimeter reads zero on landing and take off.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "QFE is the pressure at the airfield elevation. When set, the altimeter reads height above that airfield. On the runway, it reads zero.",
        },
        {
          id: 119,
          category: "Aircraft Design",
          text: "Wing loading is",
          options: [
            {
              letter: "A",
              text: "WING AREA * WING CHORD.",
            },
            {
              letter: "B",
              text: "GROSS WEIGHT divided by GROSS WING AREA.",
            },
            {
              letter: "C",
              text: "the ultimate tensile strength of the wing.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Wing loading is the aircraft's gross weight divided by the gross wing area. It is a key performance parameter.",
        },
        {
          id: 120,
          category: "Aerodynamics",
          text: "Weight is equal to",
          options: [
            {
              letter: "A",
              text: "mass * acceleration.",
            },
            {
              letter: "B",
              text: "mass * gravity.",
            },
            {
              letter: "C",
              text: "volume * gravity.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Weight is the force exerted on a mass by gravity. The standard formula is Weight = Mass * Acceleration due to gravity (W = mg).",
        },
        {
          id: 121,
          category: "Aerodynamics",
          text: "Induced drag",
          options: [
            {
              letter: "A",
              text: "increases with increase in aircraft weight.",
            },
            {
              letter: "B",
              text: "increases with an increase in speed.",
            },
            {
              letter: "C",
              text: "reduces with an increase in angle of attack.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "More weight requires more lift, and more lift generates more induced drag. Induced drag decreases with an increase in speed, and increases with an increase in angle of attack.",
        },
        {
          id: 122,
          category: "Aerodynamics",
          text: "Airflow over the upper surface of the wing generally",
          options: [
            {
              letter: "A",
              text: "flows towards the tip.",
            },
            {
              letter: "B",
              text: "flows towards the root.",
            },
            {
              letter: "C",
              text: "flows straight from leading edge to trailing edge.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The lower pressure on the upper surface creates a spanwise pressure gradient. Air from the higher pressure region near the root is drawn outwards towards the lower pressure region at the tip. Therefore, airflow on the upper surface generally flows towards the tip.",
        },
        {
          id: 123,
          category: "Aerodynamics",
          text: "With an increase in aspect ratio for a given IAS, induced drag will",
          options: [
            {
              letter: "A",
              text: "reduce.",
            },
            {
              letter: "B",
              text: "remain constant.",
            },
            {
              letter: "C",
              text: "increase.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Induced drag is inversely proportional to aspect ratio. A higher aspect ratio wing is more efficient and produces less induced drag for the same amount of lift.",
        },
        {
          id: 124,
          category: "Aerodynamics",
          text: "If the density of the air is increased, the lift will",
          options: [
            {
              letter: "A",
              text: "remain the same.",
            },
            {
              letter: "B",
              text: "increase.",
            },
            {
              letter: "C",
              text: "decrease.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Lift is directly proportional to air density (Lift = CL * ½ρV² * S). For a constant speed and angle of attack, a higher density will result in greater lift.",
        },
        {
          id: 125,
          category: "Aerodynamics",
          text: "All the factors that affect the lift produced by an aerofoil are",
          options: [
            {
              letter: "A",
              text: "angle of attack, velocity, wing area, aerofoil shape, air density.",
            },
            {
              letter: "B",
              text: "angle of attack, air temperature, velocity, wing area.",
            },
            {
              letter: "C",
              text: "angle of attack, air density, velocity, wing area.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The lift equation is L = CL * ½ρV² * S. CL is the coefficient of lift, which is affected by angle of attack and airfoil shape. ρ is air density, V is velocity, and S is wing area. Temperature is not a direct factor; its effect is through density.",
        },
        {
          id: 126,
          category: "Aerodynamics",
          text: "A wing section suitable for high speed would be",
          options: [
            {
              letter: "A",
              text: "thin with high camber.",
            },
            {
              letter: "B",
              text: "thick with high camber.",
            },
            {
              letter: "C",
              text: "thin with little or no camber.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "To delay the onset of compressibility effects and shock waves, high-speed wings are thin and have low camber. This reduces the amount of acceleration of the airflow over the wing, increasing the Critical Mach Number.",
        },
        {
          id: 127,
          category: "Aerodynamics",
          text: "The induced drag of an aircraft",
          options: [
            {
              letter: "A",
              text: "increases if aspect ratio is increased.",
            },
            {
              letter: "B",
              text: "decreases with increasing speed.",
            },
            {
              letter: "C",
              text: "increases with increasing speed.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Induced drag is inversely related to speed. As speed increases, induced drag decreases.",
        },
        {
          id: 128,
          category: "Aerodynamics",
          text: "As the speed of an aircraft increases, the profile drag",
          options: [
            {
              letter: "A",
              text: "decreases at first then increase.",
            },
            {
              letter: "B",
              text: "increases.",
            },
            {
              letter: "C",
              text: "decreases.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Profile drag (which includes form and skin friction drag) increases with the square of the speed. As speed goes up, profile drag increases significantly.",
        },
        {
          id: 129,
          category: "Aerodynamics",
          text: "The stagnation point on an aerofoil is the point where",
          options: [
            {
              letter: "A",
              text: "the boundary layer changes from laminar to turbulent.",
            },
            {
              letter: "B",
              text: "the suction pressure reaches a maximum.",
            },
            {
              letter: "C",
              text: "the airflow is brought completely to rest.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The stagnation point is located near the leading edge where the oncoming airflow divides, with some going over the wing and some under. At this exact point, the local velocity is zero, and pressure is at a maximum (total pressure).",
        },
        {
          id: 130,
          category: "Aerodynamics",
          text: "The stalling of an aerofoil is affected by the",
          options: [
            {
              letter: "A",
              text: "transition speed.",
            },
            {
              letter: "B",
              text: "airspeed.",
            },
            {
              letter: "C",
              text: "angle of attack.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "An airfoil stalls when the critical angle of attack is exceeded, regardless of airspeed. Airspeed affects the speed at which that angle is reached, but the stall itself is an angle-of-attack phenomenon.",
        },
        {
          id: 131,
          category: "Propulsion",
          text: "The most fuel efficient of the following types of engine is the",
          options: [
            {
              letter: "A",
              text: "turbo-jet engine.",
            },
            {
              letter: "B",
              text: "turbo-fan engine.",
            },
            {
              letter: "C",
              text: "rocket.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "For subsonic flight, the high-bypass turbofan engine is the most fuel-efficient of the options listed. It combines the high thrust of a jet core with the efficiency of a large, slow-moving fan.",
        },
        {
          id: 132,
          category: "Propulsion",
          text: "The quietest of the following types of engine is the",
          options: [
            {
              letter: "A",
              text: "turbo-jet engine.",
            },
            {
              letter: "B",
              text: "rocket.",
            },
            {
              letter: "C",
              text: "turbo-fan engine.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "High-bypass turbofan engines are significantly quieter than turbojets. The fan acts to slow down and mix the high-speed jet exhaust with ambient air, reducing the noise produced by the high-velocity exhaust stream.",
        },
        {
          id: 133,
          category: "Aerodynamics",
          text: "Forward motion of a glider is provided by",
          options: [
            {
              letter: "A",
              text: "the weight.",
            },
            {
              letter: "B",
              text: "the drag.",
            },
            {
              letter: "C",
              text: "the engine.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "A glider has no engine. Its forward motion is sustained by converting potential energy (altitude) into kinetic energy (speed). The weight force has a forward component along the glide path that provides the 'thrust' to overcome drag.",
        },
        {
          id: 134,
          category: "Aerodynamics",
          text: "Profile drag consists of what drag types?",
          options: [
            {
              letter: "A",
              text: "Form, induced and interference.",
            },
            {
              letter: "B",
              text: "Form, induced and skin friction.",
            },
            {
              letter: "C",
              text: "Form, skin friction and interference.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Profile drag is the sum of form drag (due to shape) and skin friction drag (due to surface roughness). Interference drag, caused by airflow interaction at wing-fuselage junctions, is often considered a separate component of parasite drag, but is sometimes grouped with profile drag.",
        },
        {
          id: 135,
          category: "Aerodynamics",
          text: "An aircraft in straight and level flight is subject to",
          options: [
            {
              letter: "A",
              text: "a load factor of 1.",
            },
            {
              letter: "B",
              text: "a load factor of ½.",
            },
            {
              letter: "C",
              text: "zero load factor.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Load factor is the ratio of lift to weight (L/W). In straight and level, unaccelerated flight, lift equals weight, so the load factor is 1.",
        },
        {
          id: 136,
          category: "Aircraft Design",
          text: "Aspect ratio is given by the formula:",
          options: [
            {
              letter: "A",
              text: "Mean Chord / Span.",
            },
            {
              letter: "B",
              text: "Span^2 / Area.",
            },
            {
              letter: "C",
              text: "Span^2 / Mean Chord.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Aspect Ratio (AR) is defined as the square of the wingspan divided by the wing area: AR = b² / S.",
        },
        {
          id: 137,
          category: "Aircraft Design",
          text: "An aspect ratio of 8 means",
          options: [
            {
              letter: "A",
              text: "the mean chord is 8 times the span.",
            },
            {
              letter: "B",
              text: "the span is 8 times the mean chord.",
            },
            {
              letter: "C",
              text: "the area is 8 times the span.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "For a rectangular wing, AR = Span / Chord. Therefore, an AR of 8 means the span is 8 times the mean chord.",
        },
        {
          id: 138,
          category: "Aerodynamics",
          text: "A high aspect ratio wing",
          options: [
            {
              letter: "A",
              text: "has a higher stall angle than a low aspect ratio wing.",
            },
            {
              letter: "B",
              text: "is stiffer than a low aspect ratio wing.",
            },
            {
              letter: "C",
              text: "has less induced drag than a low aspect ratio wing.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The primary advantage of a high aspect ratio wing is that it produces significantly less induced drag for the same amount of lift compared to a low aspect ratio wing.",
        },
        {
          id: 139,
          category: "Aerodynamics",
          text: "Induced downwash",
          options: [
            {
              letter: "A",
              text: "reduces the effective angle of attack of the wing.",
            },
            {
              letter: "B",
              text: "increases the effective angle of attack of the wing.",
            },
            {
              letter: "C",
              text: "has no effect on the angle of attack of the wing.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Downwash tilts the local relative airflow downward. This reduces the angle at which the wing section meets the air, effectively reducing its angle of attack compared to the geometric angle of attack of the aircraft.",
        },
        {
          id: 140,
          category: "Aircraft Design",
          text: "Given 2 wings, the first with a span of 12m and a chord of 2 m. The second has a span of 6m and a chord of 1m. How do their Aspect Ratios compare?",
          options: [
            {
              letter: "A",
              text: "The first is higher.",
            },
            {
              letter: "B",
              text: "They are the same.",
            },
            {
              letter: "C",
              text: "The second is higher.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Aspect Ratio = Span / Chord. For the first wing: 12 / 2 = 6. For the second wing: 6 / 1 = 6. They have the same aspect ratio.",
        },
        {
          id: 141,
          category: "Stability",
          text: "The C of G moves in flight. The most likely cause of this is",
          options: [
            {
              letter: "A",
              text: "movement of passengers.",
            },
            {
              letter: "B",
              text: "consumption of fuel and oils.",
            },
            {
              letter: "C",
              text: "movement of cargo.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "While passengers and cargo can move, the most significant and continuous cause of CG movement during flight is the consumption of fuel. Fuel is a large percentage of the aircraft's weight, and its location changes as it is used.",
        },
        {
          id: 142,
          category: "Aerodynamics",
          text: "A straight rectangular wing, without any twist, will",
          options: [
            {
              letter: "A",
              text: "stall equally along the span of the wing.",
            },
            {
              letter: "B",
              text: "stall first at the tip.",
            },
            {
              letter: "C",
              text: "stall first at the root.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "A straight, untwisted rectangular wing typically stalls at the root first. This is because the root has a higher thickness/chord ratio and experiences a slightly higher effective angle of attack due to spanwise flow.",
        },
        {
          id: 143,
          category: "Aircraft Design",
          text: "Aspect ratio of a wing is defined as the ratio of the",
          options: [
            {
              letter: "A",
              text: "wingspan to the mean chord.",
            },
            {
              letter: "B",
              text: "wingspan to the wing root.",
            },
            {
              letter: "C",
              text: "square of the chord to the wingspan.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Aspect Ratio is wingspan divided by the Mean Aerodynamic Chord (MAC).",
        },
        {
          id: 144,
          category: "Aerodynamics",
          text: "Which of the following is true?",
          options: [
            {
              letter: "A",
              text: "Lift acts at right angles to the relative airflow and weight acts vertically down.",
            },
            {
              letter: "B",
              text: "Lift acts at right angles to the wing chord line and weight acts vertically down.",
            },
            {
              letter: "C",
              text: "Lift acts at right angles to the relative air flow and weight acts at right angles to the aircraft centre line.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "By definition, lift is perpendicular to the relative airflow. Weight always acts vertically downward toward the center of the Earth.",
        },
        {
          id: 145,
          category: "Aerodynamics",
          text: "The airflow over the upper surface of a cambered wing",
          options: [
            {
              letter: "A",
              text: "increases in velocity and reduces in pressure.",
            },
            {
              letter: "B",
              text: "increases in velocity and pressure.",
            },
            {
              letter: "C",
              text: "reduces in velocity and increases in pressure.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The curved upper surface causes the airflow to accelerate. According to Bernoulli's principle, an increase in velocity results in a decrease in static pressure. This pressure reduction is a primary source of lift.",
        },
        {
          id: 146,
          category: "Aerodynamics",
          text: "With increased speed in level flight",
          options: [
            {
              letter: "A",
              text: "profile drag increases.",
            },
            {
              letter: "B",
              text: "induced drag increases.",
            },
            {
              letter: "C",
              text: "profile drag remains constant.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Profile drag, which is a component of parasite drag, increases with the square of the speed. As speed increases, profile drag increases significantly.",
        },
        {
          id: 147,
          category: "Aerodynamics",
          text: "The angle of attack of an aerofoil section is the angle between the",
          options: [
            {
              letter: "A",
              text: "underside of the wing surface and the mean airflow.",
            },
            {
              letter: "B",
              text: "chord line and the relative airflow.",
            },
            {
              letter: "C",
              text: "chord line and the centre line of the fuselage.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Angle of Attack (AoA) is specifically defined as the angle between the chord line of the airfoil and the direction of the oncoming, undisturbed airflow (relative wind).",
        },
        {
          id: 148,
          category: "Aerodynamics",
          text: "A swept wing tends to stall first at the",
          options: [
            {
              letter: "A",
              text: "centre section.",
            },
            {
              letter: "B",
              text: "root.",
            },
            {
              letter: "C",
              text: "tip.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "On a swept-back wing, the tips are the most aerodynamically rearward part. They tend to stall before the root due to spanwise flow and the redistribution of lift. This can cause a dangerous pitch-up moment.",
        },
        {
          id: 149,
          category: "Aerodynamics",
          text: "The trailing vortex on a pointed wing (taper ratio = 0) is",
          options: [
            {
              letter: "A",
              text: "at the tip.",
            },
            {
              letter: "B",
              text: "equally all along the wing span.",
            },
            {
              letter: "C",
              text: "at the root.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "A taper ratio of 0 means the tip chord is zero, resulting in a very sharp, pointed tip. The trailing vortex is concentrated at the tip, as that is where the pressure differential between the upper and lower surfaces ceases to exist.",
        },
        {
          id: 150,
          category: "Aerodynamics",
          text: "The lift curve for a delta wing is",
          options: [
            {
              letter: "A",
              text: "more steep than that of a high aspect ratio wing.",
            },
            {
              letter: "B",
              text: "less steep than that of a high aspect ratio wing.",
            },
            {
              letter: "C",
              text: "the same as that of a high aspect ratio wing.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "A delta wing generates lift not only from conventional potential flow but also from a powerful leading-edge vortex that increases lift, especially at high angles of attack. This results in a steeper lift curve slope (dCL/dα) compared to a conventional high aspect ratio wing.",
        },
        {
          id: 151,
          category: "Aerodynamics",
          text: "A delta wing has",
          options: [
            {
              letter: "A",
              text: "a lower stall angle than a straight wing.",
            },
            {
              letter: "B",
              text: "a higher stall angle than a straight wing.",
            },
            {
              letter: "C",
              text: "the same stall angle than a straight wing.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Delta wings, due to the generation of leading-edge vortices, can maintain controlled flight and lift production at very high angles of attack, often exceeding 30-40 degrees. This is much higher than the stall angle of a typical straight wing (around 15-18 degrees).",
        },
        {
          id: 152,
          category: "Aerodynamics",
          text: "The speed of air over a swept wing which contributes to the lift is",
          options: [
            {
              letter: "A",
              text: "less than the aircraft speed.",
            },
            {
              letter: "B",
              text: "the same as the aircraft speed.",
            },
            {
              letter: "C",
              text: "more than the aircraft speed.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Only the component of airflow perpendicular to the leading edge of a swept wing is effective in producing lift. This component is equal to the free-stream velocity multiplied by the cosine of the sweep angle, which is always less than the aircraft's true airspeed.",
        },
        {
          id: 153,
          category: "Aerodynamics",
          text: "For a given angle of attack, induced drag is",
          options: [
            {
              letter: "A",
              text: "greater on a high aspect ratio wing.",
            },
            {
              letter: "B",
              text: "greater towards the wing root.",
            },
            {
              letter: "C",
              text: "greater on a low aspect ratio wing.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Induced drag is inversely proportional to aspect ratio. For the same angle of attack (and thus lift coefficient), a low aspect ratio wing will produce significantly more induced drag.",
        },
        {
          id: 154,
          category: "Aerodynamics",
          text: "In straight and level flight, the angle of attack of a swept wing is",
          options: [
            {
              letter: "A",
              text: "less than the aircraft angle to the horizontal.",
            },
            {
              letter: "B",
              text: "more than the aircraft angle to the horizontal.",
            },
            {
              letter: "C",
              text: "the same as the aircraft angle to the horizontal.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The aircraft's angle to the horizontal is its pitch attitude. Due to wing twist (washout) and the fact that only a component of the freestream velocity acts over the wing, the local angle of attack on a swept wing is typically less than the aircraft's pitch attitude.",
        },
        {
          id: 155,
          category: "Aerodynamics",
          text: "Induced drag",
          options: [
            {
              letter: "A",
              text: "is equal to the profile drag at Vmd.",
            },
            {
              letter: "B",
              text: "is equal to the profile drag at the stalling speed.",
            },
            {
              letter: "C",
              text: "is never equal to the profile drag.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "At the speed for minimum total drag (Vmd), the two main components of total drag—induced drag and profile drag—are equal.",
        },
        {
          id: 156,
          category: "Aerodynamics",
          text: "A delta wing aircraft flying at the same speed (subsonic) and angle of attack as a swept wing aircraft of similar wing area will produce",
          options: [
            {
              letter: "A",
              text: "more lift.",
            },
            {
              letter: "B",
              text: "less lift.",
            },
            {
              letter: "C",
              text: "the same lift.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "For the same area, speed, and angle of attack, a delta wing produces more lift. This is due to the additional lift generated by the powerful vortices that form along its sharp leading edges at moderate to high angles of attack.",
        },
        {
          id: 157,
          category: "Aerodynamics",
          text: "The stagnation point is",
          options: [
            {
              letter: "A",
              text: "static pressure minus dynamic pressure.",
            },
            {
              letter: "B",
              text: "dynamic pressure only.",
            },
            {
              letter: "C",
              text: "static pressure plus dynamic pressure.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "At the stagnation point, the velocity is zero, so the dynamic pressure is zero. The pressure here is the total pressure, which is the sum of the static pressure and the dynamic pressure of the free stream (Ptotal = Pstatic + ½ρV²).",
        },
        {
          id: 158,
          category: "Aerodynamics",
          text: "On a swept wing aircraft, due to the adverse pressure gradient, the boundary layer on the upper surface of the wing tends to flow",
          options: [
            {
              letter: "A",
              text: "towards the root.",
            },
            {
              letter: "B",
              text: "towards the tip.",
            },
            {
              letter: "C",
              text: "directly from leading edge to trailing edge.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "On a swept wing, there is a strong spanwise pressure gradient from the tip (lower pressure) to the root (higher pressure). The boundary layer, being low energy, is susceptible to this gradient and flows inboard towards the root, thickening it and contributing to tip stall.",
        },
        {
          id: 159,
          category: "Aerodynamics",
          text: "With increased speed in level flight",
          options: [
            {
              letter: "A",
              text: "induced drag increases.",
            },
            {
              letter: "B",
              text: "profile drag increases.",
            },
            {
              letter: "C",
              text: "profile drag remains constant.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Profile drag is a function of dynamic pressure (½ρV²). As speed increases, dynamic pressure increases, and so does profile drag. Induced drag, conversely, decreases.",
        },
        {
          id: 160,
          category: "Aerodynamics",
          text: "If a swept wing stalls at the tips first, the aircraft will",
          options: [
            {
              letter: "A",
              text: "pitch nose up.",
            },
            {
              letter: "B",
              text: "roll.",
            },
            {
              letter: "C",
              text: "pitch nose down.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "When the tips stall first, the center of lift (CP) moves forward on the wing. This forward movement of the CP ahead of the center of gravity creates a nose-up pitching moment, which can exacerbate the stall and lead to a dangerous condition known as 'pitch-up'.",
        },
        {
          id: 161,
          category: "Aircraft Design",
          text: "The thickness/chord ratio of the wing is also known as",
          options: [
            {
              letter: "A",
              text: "fineness ratio.",
            },
            {
              letter: "B",
              text: "mean chord ratio.",
            },
            {
              letter: "C",
              text: "aspect ratio.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The fineness ratio is a term used to describe the slenderness of a body. For an airfoil, it is the ratio of its maximum thickness to its chord length (t/c).",
        },
        {
          id: 162,
          category: "Aerodynamics",
          text: "Flexure of a rearward swept wing will",
          options: [
            {
              letter: "A",
              text: "increase the lift and hence increase the flexure.",
            },
            {
              letter: "B",
              text: "increase the lift and hence decrease the flexure.",
            },
            {
              letter: "C",
              text: "decrease the lift and hence decrease the flexure.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "When a swept wing flexes under load, the tips twist and bend upward. This upward bending effectively reduces the angle of attack at the tips. However, more importantly, it can also reduce the effective sweep angle of the outboard sections, causing them to produce more lift, which increases the flexure further. This is known as aileron reversal or structural divergence.",
        },
        {
          id: 163,
          category: "Aircraft Design",
          text: "A High Aspect Ratio wing is a wing with",
          options: [
            {
              letter: "A",
              text: "short span, long chord.",
            },
            {
              letter: "B",
              text: "long span, long chord.",
            },
            {
              letter: "C",
              text: "long span, short chord.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "A high aspect ratio wing is long and slender, meaning it has a long span and a short chord. Gliders are a classic example.",
        },
        {
          id: 164,
          category: "Aerodynamics",
          text: "Stall commencing at the root is preferred because",
          options: [
            {
              letter: "A",
              text: "it provides the pilot with a warning of complete loss of lift.",
            },
            {
              letter: "B",
              text: "the ailerons become ineffective.",
            },
            {
              letter: "C",
              text: "it will cause the aircraft to pitch nose up.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "When the root stalls first, the turbulent airflow causes the aircraft to buffet (shake), warning the pilot of an impending stall. Crucially, the ailerons near the tips are still in smooth air and remain effective for lateral control.",
        },
        {
          id: 165,
          category: "Aerodynamics",
          text: "If the angle of attack of a wing is increased in flight, the",
          options: [
            {
              letter: "A",
              text: "CofP will move aft.",
            },
            {
              letter: "B",
              text: "CofP will move forward.",
            },
            {
              letter: "C",
              text: "C of G will move aft.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "For a cambered airfoil, increasing the angle of attack causes the center of pressure to move forward along the chord. (Note: CG does not move with AoA).",
        },
        {
          id: 166,
          category: "Aerodynamics",
          text: "The Rams Horn Vortex on a forward swept wing will be",
          options: [
            {
              letter: "A",
              text: "more than a rearward swept wing.",
            },
            {
              letter: "B",
              text: "less than a rearward swept wing.",
            },
            {
              letter: "C",
              text: "the same as a rearward swept wing.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Forward-swept wings are prone to much stronger and more complex vortex formations at the wing-body junction (often called a 'ram's horn' vortex) compared to rearward-swept wings, contributing to their aerodynamic challenges.",
        },
        {
          id: 167,
          category: "Aerodynamics",
          text: "For a cambered wing section the zero lift angle of attack will be",
          options: [
            {
              letter: "A",
              text: "4 degrees.",
            },
            {
              letter: "B",
              text: "zero.",
            },
            {
              letter: "C",
              text: "negative.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "A cambered airfoil generates lift even at a zero angle of attack. To produce zero lift, the airfoil must be angled downwards, i.e., have a negative angle of attack.",
        },
        {
          id: 168,
          category: "Aerodynamics",
          text: "Airflow at subsonic speed is taken to be",
          options: [
            {
              letter: "A",
              text: "compressible.",
            },
            {
              letter: "B",
              text: "either a or b depending on altitude.",
            },
            {
              letter: "C",
              text: "incompressible.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "For basic aerodynamic theory and performance calculations at low speeds (typically below Mach 0.3 or 0.4), air is assumed to be incompressible. This simplifies the equations significantly. At higher subsonic speeds (approaching Mach 1), compressibility effects become important.",
        },
        {
          id: 169,
          category: "Aerodynamics",
          text: "If fluid flow through a venturi is said to be incompressible, the speed of the flow increases at the throat to",
          options: [
            {
              letter: "A",
              text: "allow for a reduction in static pressure.",
            },
            {
              letter: "B",
              text: "maintain a constant volume flow rate.",
            },
            {
              letter: "C",
              text: "allow for an increase in static pressure.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The principle of continuity for incompressible flow states that mass flow rate is constant. Since density is constant, the volume flow rate (A * V) must also be constant. Therefore, when the cross-sectional area (A) decreases at the throat, the velocity (V) must increase to maintain a constant volume flow.",
        },
        {
          id: 170,
          category: "Aerodynamics",
          text: "To produce lift, an aerofoil must be",
          options: [
            {
              letter: "A",
              text: "asymmetrical.",
            },
            {
              letter: "B",
              text: "symmetrical.",
            },
            {
              letter: "C",
              text: "either symmetrical or asymmetrical.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Both symmetrical and asymmetrical (cambered) airfoils can produce lift. A symmetrical airfoil produces lift by being presented to the airflow at a positive angle of attack. A cambered airfoil can produce lift even at zero angle of attack.",
        },
        {
          id: 171,
          category: "Aerodynamics",
          text: "Lift is dependent on",
          options: [
            {
              letter: "A",
              text: "the net area of the wing ,the density of the fluid medium and the velocity.",
            },
            {
              letter: "B",
              text: "the area of the wing, the density of the fluid medium, and the square of the velocity.",
            },
            {
              letter: "C",
              text: "the frontal area of the wing, the density of the fluid medium and the velocity.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The lift equation is L = CL * ½ρV² * S. This shows lift is directly proportional to wing area (S), air density (ρ), and the square of the velocity (V²). Frontal area is more related to drag.",
        },
        {
          id: 172,
          category: "Aerodynamics",
          text: "A wing develops 10,000 N of lift at 100 knots. Assuming the wing remains at the same angle of attack and remains at the same altitude, how much lift will it develop at 300knots?",
          options: [
            {
              letter: "A",
              text: "30,000 N.",
            },
            {
              letter: "B",
              text: "900,000 N.",
            },
            {
              letter: "C",
              text: "90,000 N.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Lift is proportional to the square of the velocity (V²). If speed triples (from 100 to 300 knots), lift increases by a factor of 3² = 9. Therefore, 10,000 N * 9 = 90,000 N.",
        },
        {
          id: 173,
          category: "Aerodynamics",
          text: "The angle of attack is",
          options: [
            {
              letter: "A",
              text: "related to angle of incidence.",
            },
            {
              letter: "B",
              text: "always kept below 15 degrees.",
            },
            {
              letter: "C",
              text: "not related to the angle of incidence.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Angle of attack is the angle between the chord line and the relative wind. Angle of incidence is the fixed angle between the chord line and the aircraft's longitudinal axis. They are completely independent of each other.",
        },
        {
          id: 174,
          category: "Aerodynamics",
          text: "The difference between the mean camber line and the chord line of an aerofoil is",
          options: [
            {
              letter: "A",
              text: "neither are straight.",
            },
            {
              letter: "B",
              text: "they both may be curved.",
            },
            {
              letter: "C",
              text: "one is always straight and the other may be straight.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The chord line is always a straight line from the leading edge to the trailing edge. The mean camber line is a curve that lies halfway between the upper and lower surfaces. For a symmetrical airfoil, the mean camber line is straight and coincides with the chord line.",
        },
        {
          id: 175,
          category: "Stability",
          text: "If the C of G is calculated after loading as within limits for take off",
          options: [
            {
              letter: "A",
              text: "a further calculation is required prior to landing to allow for fuel and oil consumption.",
            },
            {
              letter: "B",
              text: "a further calculation is required prior to landing to allow for flap deployment.",
            },
            {
              letter: "C",
              text: "no further calculation is required.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "CG position changes during flight, primarily due to fuel consumption. As fuel is burned, the CG can shift. It is essential to ensure that the CG remains within limits for landing as well, so a landing CG calculation must be performed.",
        },
        {
          id: 176,
          category: "Aerodynamics",
          text: "The span wise component of the airflow is",
          options: [
            {
              letter: "A",
              text: "greater at higher speeds.",
            },
            {
              letter: "B",
              text: "unaffected by speed.",
            },
            {
              letter: "C",
              text: "less at higher speeds.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Spanwise flow is driven by the pressure differential between the root and tip. At higher speeds, the angle of attack is lower, which reduces the pressure difference and the strength of the vortices, thus reducing the spanwise component of flow.",
        },
        {
          id: 177,
          category: "Aircraft Design",
          text: "A wing fence",
          options: [
            {
              letter: "A",
              text: "acts as a lift dumping device.",
            },
            {
              letter: "B",
              text: "reduces span wise flow on a swept wing thus reducing induced drag.",
            },
            {
              letter: "C",
              text: "increases lateral control.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "A wing fence is a flat plate on the upper surface of a swept wing, running chordwise. It blocks the spanwise flow of the boundary layer, preventing it from migrating to the tip and thickening. This helps delay tip stall and can slightly reduce induced drag.",
        },
        {
          id: 178,
          category: "Aerodynamics",
          text: "With all conditions remaining the same, if the aircraft speed is halved, by what factor is the lift reduced?",
          options: [
            {
              letter: "A",
              text: "Half.",
            },
            {
              letter: "B",
              text: "By a factor of 4.",
            },
            {
              letter: "C",
              text: "Remains the same.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Lift is proportional to the square of the velocity. If speed is halved (multiplied by 1/2), lift is multiplied by (1/2)² = 1/4. Therefore, lift is reduced by a factor of 4.",
        },
        {
          id: 179,
          category: "Aerodynamics",
          text: "The boundary layer over an aerofoil is",
          options: [
            {
              letter: "A",
              text: "a layer of air close to the aerofoil which is moving at a velocity less than free stream air.",
            },
            {
              letter: "B",
              text: "a layer of turbulent air close to the aerofoil which is moving at a velocity less than free stream air.",
            },
            {
              letter: "C",
              text: "a layer of air close to the aerofoil that is stationary.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The boundary layer is the thin region where viscous forces are significant. The air velocity ranges from zero at the surface (no-slip condition) up to the free-stream velocity at the edge of the layer. It can be laminar or turbulent.",
        },
        {
          id: 180,
          category: "Aircraft Design",
          text: "On a swept wing aircraft, the fineness ratio of an aerofoil is",
          options: [
            {
              letter: "A",
              text: "highest at the root.",
            },
            {
              letter: "B",
              text: "equal throughout the span.",
            },
            {
              letter: "C",
              text: "highest at the tip.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Fineness ratio is the thickness/chord ratio (t/c). On swept wings, the root is often thicker to house landing gear and provide structural strength. Therefore, the fineness ratio is highest at the root.",
        },
        {
          id: 181,
          category: "Aerodynamics",
          text: "Streamlining will reduce",
          options: [
            {
              letter: "A",
              text: "induced drag.",
            },
            {
              letter: "B",
              text: "skin friction drag.",
            },
            {
              letter: "C",
              text: "form drag.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Streamlining is the process of shaping a body to reduce the size of the turbulent wake behind it. A smaller wake results in lower pressure drag, which is the dominant component of form drag.",
        },
        {
          id: 182,
          category: "Aerodynamics",
          text: "If an aircraft has a gross weight of 3000 kg and is then subjected to a total weight of 6000 kg the load factor will be",
          options: [
            {
              letter: "A",
              text: "2G.",
            },
            {
              letter: "B",
              text: "9G.",
            },
            {
              letter: "C",
              text: "3G.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Load factor (n) is Lift / Weight. If the total force supporting the aircraft (Lift) is 6000 kg and its actual weight is 3000 kg, the load factor is 6000/3000 = 2.",
        },
        {
          id: 183,
          category: "Aerodynamics",
          text: "Ice formed on the leading edge will cause the aircraft to",
          options: [
            {
              letter: "A",
              text: "stall at a higher speed.",
            },
            {
              letter: "B",
              text: "stall at a lower speed.",
            },
            {
              letter: "C",
              text: "stall at the same stall speed and AOA.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Ice on the leading edge destroys the smooth airflow over the wing. It causes the boundary layer to separate earlier, at a lower angle of attack. This means the wing will stall at a higher speed and at a lower angle of attack than normal.",
        },
        {
          id: 184,
          category: "Aerodynamics",
          text: "Under what conditions will an aircraft create best lift?",
          options: [
            {
              letter: "A",
              text: "Hot damp day at 1200 ft.",
            },
            {
              letter: "B",
              text: "Cold dry day at 200 ft.",
            },
            {
              letter: "C",
              text: "Cold wet day at 1200 ft.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Lift is directly proportional to air density. Best lift is achieved when density is highest. This occurs on a cold day (low temperature), at low altitude (high pressure), and in dry air (water vapor is less dense than dry air, so dry air is denser).",
        },
        {
          id: 185,
          category: "Aerodynamics",
          text: "As Mach number increases, what is the effect on boundary layer?",
          options: [
            {
              letter: "A",
              text: "Becomes more turbulent.",
            },
            {
              letter: "B",
              text: "Decreases in thickness.",
            },
            {
              letter: "C",
              text: "Becomes less turbulent.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "As Mach number increases, especially into the transonic region, shock waves form. The interaction between shock waves and the boundary layer causes adverse pressure gradients, which make the boundary layer thicker, more turbulent, and prone to separation.",
        },
        {
          id: 186,
          category: "Aerodynamics",
          text: "During a glide the following forces act on an aircraft",
          options: [
            {
              letter: "A",
              text: "lift and weight only.",
            },
            {
              letter: "B",
              text: "lift, drag, weight.",
            },
            {
              letter: "C",
              text: "lift, weight, thrust.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "In a glide, the engine is at idle or off, so there is no significant thrust. However, the aircraft is still moving, so Lift, Weight, and Drag are all present. The Weight vector has a component that acts forward along the flight path to balance Drag.",
        },
        {
          id: 187,
          category: "Aerodynamics",
          text: "If an aileron is moved downward",
          options: [
            {
              letter: "A",
              text: "the stalling angle of that wing is increased.",
            },
            {
              letter: "B",
              text: "the stalling angle is not affected but the stalling speed is decreased.",
            },
            {
              letter: "C",
              text: "the stalling angle of that wing is decreased.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "A downward-deflected aileron increases the camber and angle of attack of that portion of the wing. This brings that wing section closer to its critical angle of attack, effectively reducing its stall margin and stalling angle.",
        },
        {
          id: 188,
          category: "Aerodynamics",
          text: "If the wing loading of an aircraft were reduced the stalling speed would",
          options: [
            {
              letter: "A",
              text: "increase.",
            },
            {
              letter: "B",
              text: "not be affected.",
            },
            {
              letter: "C",
              text: "decrease.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Wing loading is Weight/Area. Stalling speed (Vs) is proportional to the square root of wing loading (Vs ∝ √(Wing Loading)). If wing loading is reduced (less weight or more area), the stalling speed decreases.",
        },
        {
          id: 189,
          category: "Aerodynamics",
          text: "The lift on a wing is increased with",
          options: [
            {
              letter: "A",
              text: "an increase in temperature.",
            },
            {
              letter: "B",
              text: "an increase in pressure.",
            },
            {
              letter: "C",
              text: "an increase in humidity.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Lift is directly proportional to air density. An increase in pressure, at constant temperature, increases air density. Therefore, lift increases. Increased temperature or humidity decreases density, thus decreasing lift.",
        },
        {
          id: 190,
          category: "Aerodynamics",
          text: "The airflow behind a normal shockwave will",
          options: [
            {
              letter: "A",
              text: "always be subsonic and in the same direction as the original airflow.",
            },
            {
              letter: "B",
              text: "always be supersonic and in the same direction as the original airflow.",
            },
            {
              letter: "C",
              text: "always be subsonic and deflected from the direction of the original airflow.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "A key property of a normal shock wave is that it decelerates the flow from supersonic to subsonic. The flow direction remains unchanged (normal to the shock wave).",
        },
        {
          id: 191,
          category: "Aerodynamics",
          text: "Induced drag can be reduced by the use of",
          options: [
            {
              letter: "A",
              text: "streamlining.",
            },
            {
              letter: "B",
              text: "high aspect ratio wings.",
            },
            {
              letter: "C",
              text: "fairings at junctions between fuselage and wings.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Induced drag is a direct result of generating lift. A high aspect ratio wing (long and slender) produces the same lift with a smaller wingtip vortex and less downwash, thereby significantly reducing induced drag.",
        },
        {
          id: 192,
          category: "Aerodynamics",
          text: "Interference drag can be reduced by the use of",
          options: [
            {
              letter: "A",
              text: "fairings at junctions between fuselage and wings.",
            },
            {
              letter: "B",
              text: "high aspect ratio wings.",
            },
            {
              letter: "C",
              text: "streamlining.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Interference drag occurs where two surfaces meet (e.g., wing-fuselage, fin-tailplane), as their individual boundary layers interact. Fairings are smooth, curved fairings placed at these junctions to streamline the airflow and reduce this interaction and the resulting drag.",
        },
        {
          id: 193,
          category: "Aerodynamics",
          text: "Gliding angle is the angle between",
          options: [
            {
              letter: "A",
              text: "ground and the glide path.",
            },
            {
              letter: "B",
              text: "aircraft and flight path.",
            },
            {
              letter: "C",
              text: "aircraft and airflow.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The gliding angle, or glide slope, is the angle between the flight path of the aircraft (through the air mass) and the horizontal ground. It is a measure of the aircraft's descent angle.",
        },
        {
          id: 194,
          category: "Aerodynamics",
          text: "Lift is generated by a wing",
          options: [
            {
              letter: "A",
              text: "mostly on the bottom surface.",
            },
            {
              letter: "B",
              text: "mostly on the top surface.",
            },
            {
              letter: "C",
              text: "equally on the top and bottom surfaces.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "While lift is the result of a pressure difference, approximately two-thirds of the net lift force is generated by the lower pressure (suction) on the upper surface, with the remaining one-third coming from the higher pressure on the lower surface.",
        },
        {
          id: 195,
          category: "Aerodynamics",
          text: "Lift is dependent on",
          options: [
            {
              letter: "A",
              text: "the area of the wing, the density of the fluid medium and the square of the velocity.",
            },
            {
              letter: "B",
              text: "the net area of the wing, the density of the fluid medium and the velocity.",
            },
            {
              letter: "C",
              text: "the frontal area of the wing, the density of the fluid medium and the velocity.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The lift equation L = ½ρV² * S * CL explicitly shows lift is proportional to wing area (S), density (ρ), and the square of the velocity (V²).",
        },
        {
          id: 196,
          category: "Aerodynamics",
          text: "To produce lift, an aerofoil must be",
          options: [
            {
              letter: "A",
              text: "symmetrical.",
            },
            {
              letter: "B",
              text: "asymmetrical.",
            },
            {
              letter: "C",
              text: "either symmetrical or asymmetrical.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Both symmetrical and asymmetrical (cambered) airfoils produce lift. A symmetrical airfoil requires a positive angle of attack to generate lift. A cambered airfoil can generate lift even at zero angle of attack.",
        },
        {
          id: 197,
          category: "Aerodynamics",
          text: "Airflow at sub-sonic speed is taken to be",
          options: [
            {
              letter: "A",
              text: "incompressible.",
            },
            {
              letter: "B",
              text: "compressible.",
            },
            {
              letter: "C",
              text: "either (a) or (b) depending on altitude.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "At low subsonic speeds (typically below Mach 0.3), air is treated as incompressible. At higher subsonic speeds, as the aircraft approaches the transonic region (Mach 0.7+), compressibility effects become significant and must be considered. Altitude affects the speed of sound and thus the Mach number for a given TAS.",
        },
        {
          id: 198,
          category: "Aerodynamics",
          text: "The total drag of an aircraft",
          options: [
            {
              letter: "A",
              text: "changes with speed.",
            },
            {
              letter: "B",
              text: "increases with speed.",
            },
            {
              letter: "C",
              text: "increases with the square of speed.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Total drag changes with speed. It does not simply increase with speed. At low speeds, induced drag is high and total drag is high. As speed increases, total drag decreases to a minimum (Vmd), and then increases at higher speeds due to the rapid rise in parasite drag.",
        },
        {
          id: 199,
          category: "Aerodynamics",
          text: "_______ angle of attack is known as optimum angle of attack.",
          options: [
            {
              letter: "A",
              text: "5 to 7 degrees.",
            },
            {
              letter: "B",
              text: "3 to 4 degrees.",
            },
            {
              letter: "C",
              text: "10 to 12 degrees.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The optimum angle of attack is the one that yields the maximum Lift/Drag ratio. For most conventional airfoils, this occurs at a relatively low angle of attack, typically around 3 to 5 degrees.",
        },
        {
          id: 200,
          category: "Aerodynamics",
          text: "Induced drag is",
          options: [
            {
              letter: "A",
              text: "lowest at root.",
            },
            {
              letter: "B",
              text: "greatest at tip.",
            },
            {
              letter: "C",
              text: "neutral.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Induced drag is a direct result of downwash, which is strongest near the wingtips due to the wingtip vortices. Therefore, induced drag is greatest towards the wing tips.",
        },
        {
          id: 201,
          category: "Aerodynamics",
          text: "Profile drag is",
          options: [
            {
              letter: "A",
              text: "neutral.",
            },
            {
              letter: "B",
              text: "inversely proportional.",
            },
            {
              letter: "C",
              text: "proportional to speed.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Profile drag (a component of parasite drag) is proportional to the square of the speed. As speed increases, profile drag increases significantly.",
        },
        {
          id: 202,
          category: "Aerodynamics",
          text: "A shock stall occurs at",
          options: [
            {
              letter: "A",
              text: "large angles of attack.",
            },
            {
              letter: "B",
              text: "small angles of attack.",
            },
            {
              letter: "C",
              text: "equally both large and small angles of attack.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "A shock stall is not the same as a low-speed stall. It is a high-speed phenomenon. It occurs at high Mach numbers (transonic speeds) when shock waves form on the wing, causing flow separation and a loss of lift. This typically happens at relatively low angles of attack.",
        },
        {
          id: 203,
          category: "Aerodynamics",
          text: "What happens to the wingtip stagnation point as the AOA increases?",
          options: [
            {
              letter: "A",
              text: "It moves down and under the leading edge.",
            },
            {
              letter: "B",
              text: "It moves up and over the leading edge.",
            },
            {
              letter: "C",
              text: "It remains unchanged.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "As angle of attack increases, the stagnation point, which is the point where the airflow divides, moves further down and aft along the lower surface of the leading edge.",
        },
        {
          id: 204,
          category: "Aerodynamics",
          text: "The point at which airflow ceases to be laminar and becomes turbulent is the",
          options: [
            {
              letter: "A",
              text: "boundary point.",
            },
            {
              letter: "B",
              text: "transition point.",
            },
            {
              letter: "C",
              text: "separation point.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "This is the definition of the transition point. It is the location on the surface where the boundary layer changes from a smooth laminar flow to a chaotic turbulent flow.",
        },
        {
          id: 205,
          category: "Aerodynamics",
          text: "Which of the following is true about Profile drag?",
          options: [
            {
              letter: "A",
              text: "Profile drag = Skin Drag + Form Drag.",
            },
            {
              letter: "B",
              text: "Profile drag = skin drag + induced drag.",
            },
            {
              letter: "C",
              text: "Profile drag = induced drag + Form drag.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Profile drag is the sum of skin friction drag and form (pressure) drag. It is the drag due to the shape and surface of the body itself, excluding induced drag.",
        },
        {
          id: 206,
          category: "Aerodynamics",
          text: "Which statement is true?",
          options: [
            {
              letter: "A",
              text: "Profile drag increases with the square of the airspeed.",
            },
            {
              letter: "B",
              text: "Both Induced drag and profile drag increase with the square of the airspeed.",
            },
            {
              letter: "C",
              text: "Induced drag increases with the square of the airspeed.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Profile drag is proportional to V². Induced drag is inversely proportional to V². Therefore, only profile drag increases with the square of the airspeed.",
        },
        {
          id: 207,
          category: "Aerodynamics",
          text: "Which statement is true?",
          options: [
            {
              letter: "A",
              text: "Rectangular wings stall at the root first.",
            },
            {
              letter: "B",
              text: "Both tapered and rectangular wings will stall at the tip first.",
            },
            {
              letter: "C",
              text: "Tapered wings stall at the root first.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Rectangular wings, due to their constant chord and root effects, tend to stall at the root first. Tapered wings, especially with high taper, are more prone to stalling at the tip first, which is why washout is often used.",
        },
        {
          id: 208,
          category: "Aerodynamics",
          text: "During inverted level flight an aircraft accelerometer shows",
          options: [
            {
              letter: "A",
              text: "-2g.",
            },
            {
              letter: "B",
              text: "-1g.",
            },
            {
              letter: "C",
              text: "0g.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "In inverted level flight, the lift force is acting downwards (opposite to normal) to keep the aircraft in the air. The load factor (n) is Lift/Weight. Since lift is acting opposite to the normal direction, it is considered negative, but the magnitude required is equal to weight, so n = -1g.",
        },
        {
          id: 209,
          category: "Aerodynamics",
          text: "During straight and level flight an aircraft accelerometer shows",
          options: [
            {
              letter: "A",
              text: "4g.",
            },
            {
              letter: "B",
              text: "1g.",
            },
            {
              letter: "C",
              text: "2g.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "In straight and level, unaccelerated flight, Lift equals Weight. The accelerometer measures the load factor (Lift/Weight), which is 1.",
        },
        {
          id: 210,
          category: "Aerodynamics",
          text: "Which of the following is incorrect about induced drag?",
          options: [
            {
              letter: "A",
              text: "It will increase inversely to the square of the airspeed.",
            },
            {
              letter: "B",
              text: "It will decrease in proportion to the square of the airspeed.",
            },
            {
              letter: "C",
              text: "It will increase when the angle of attack is reduced.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Induced drag decreases when angle of attack is reduced. A lower angle of attack (at higher speeds) means less lift is being generated, and therefore less induced drag. Statements A and B are equivalent and correct ways of saying induced drag is inversely proportional to V².",
        },
        {
          id: 211,
          category: "Aerodynamics",
          text: "What produces the most lift at low speeds?",
          options: [
            {
              letter: "A",
              text: "High camber.",
            },
            {
              letter: "B",
              text: "Low aspect ratio.",
            },
            {
              letter: "C",
              text: "High aspect ratio.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "High camber increases the maximum lift coefficient (CLmax) of an airfoil, allowing it to generate more lift at a given low speed. High aspect ratio improves efficiency (less drag) but doesn't directly increase the maximum lift produced.",
        },
        {
          id: 212,
          category: "Aerodynamics",
          text: "If the angle of attack is zero, but lift is produced, the",
          options: [
            {
              letter: "A",
              text: "wing is symmetrical.",
            },
            {
              letter: "B",
              text: "wing is cambered.",
            },
            {
              letter: "C",
              text: "wing has positive angle of incidence.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "A cambered (asymmetrical) airfoil can generate lift at zero angle of attack due to its curved shape. A symmetrical airfoil produces zero lift at zero AoA.",
        },
        {
          id: 213,
          category: "Aerodynamics",
          text: "When is the angle of incidence the same as the angle of attack?",
          options: [
            {
              letter: "A",
              text: "Never.",
            },
            {
              letter: "B",
              text: "In descent.",
            },
            {
              letter: "C",
              text: "When relative airflow is parallel to longitudinal axis.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Angle of incidence is the angle between the wing chord and the longitudinal axis. Angle of attack is the angle between the wing chord and the relative wind. They are equal when the relative wind is parallel to the longitudinal axis, meaning the aircraft's flight path is aligned with its nose attitude.",
        },
        {
          id: 214,
          category: "Aerodynamics",
          text: "Flaps at landing position",
          options: [
            {
              letter: "A",
              text: "decrease landing speed.",
            },
            {
              letter: "B",
              text: "decrease take off and landing speeds.",
            },
            {
              letter: "C",
              text: "decrease take off speed.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The primary purpose of landing flaps is to allow the aircraft to fly at a slower speed without stalling. They increase CLmax, which lowers the stall speed, thus decreasing the landing speed. (They are not typically used for takeoff in the same 'landing' configuration as they create high drag).",
        },
        {
          id: 215,
          category: "Aerodynamics",
          text: "As a subsonic aircraft speeds-up, its Centre of Pressure",
          options: [
            {
              letter: "A",
              text: "moves aft.",
            },
            {
              letter: "B",
              text: "moves forward.",
            },
            {
              letter: "C",
              text: "is unaffected.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Centre of Pressure movement is primarily a function of angle of attack, not speed. As long as the angle of attack is constant, the CP remains in the same relative position on the chord, regardless of speed. (Transonic effects are an exception).",
        },
        {
          id: 216,
          category: "Aerodynamics",
          text: "Lowering of the flaps",
          options: [
            {
              letter: "A",
              text: "increases drag.",
            },
            {
              letter: "B",
              text: "increases lift.",
            },
            {
              letter: "C",
              text: "increases drag and lift.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Lowering flaps increases both the camber and often the area of the wing, which increases the maximum lift coefficient (CLmax). However, it also significantly increases form drag due to the change in shape and the creation of a larger turbulent wake.",
        },
        {
          id: 217,
          category: "Aircraft Controls",
          text: "Wing spoilers, when used asymmetrically, are associated with",
          options: [
            {
              letter: "A",
              text: "rudder.",
            },
            {
              letter: "B",
              text: "elevators.",
            },
            {
              letter: "C",
              text: "ailerons.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "When used asymmetrically, spoilers are raised on one wing only. This kills lift and increases drag on that wing, causing the aircraft to roll towards that side. They are used to augment or replace ailerons for lateral control.",
        },
        {
          id: 218,
          category: "Aircraft Controls",
          text: "What do ruddervators do?",
          options: [
            {
              letter: "A",
              text: "Control yaw and roll.",
            },
            {
              letter: "B",
              text: "Control pitch and yaw.",
            },
            {
              letter: "C",
              text: "Control pitch and roll.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Ruddervators are the combined rudder and elevator on a V-tail aircraft. When moved together (both up or both down), they act as elevators, controlling pitch. When moved differentially (one up, one down), they act as a rudder, controlling yaw.",
        },
        {
          id: 219,
          category: "Aircraft Controls",
          text: "What controls pitch and roll on a delta wing aircraft?",
          options: [
            {
              letter: "A",
              text: "Ailerons.",
            },
            {
              letter: "B",
              text: "Elevons.",
            },
            {
              letter: "C",
              text: "Elevators.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Delta wing aircraft often lack separate horizontal tail surfaces. They use elevons, which are control surfaces on the trailing edge of the wing. When moved together, they control pitch; when moved differentially, they control roll.",
        },
        {
          id: 220,
          category: "Aircraft Controls",
          text: "What does a trim tab do?",
          options: [
            {
              letter: "A",
              text: "Allows the C of G to be outside the normal limit.",
            },
            {
              letter: "B",
              text: "Provides finer control movements by the pilot.",
            },
            {
              letter: "C",
              text: "Eases control loading for pilot.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Trim tabs are small, adjustable surfaces on the trailing edge of primary controls. Their purpose is to aerodynamically zero out the control forces, allowing the pilot to fly 'hands-off' without having to maintain constant pressure on the controls.",
        },
        {
          id: 221,
          category: "Aircraft Controls",
          text: "How does a balance tab move?",
          options: [
            {
              letter: "A",
              text: "In the same direction a small amount.",
            },
            {
              letter: "B",
              text: "In the opposite direction proportional to the control surface it is attached to.",
            },
            {
              letter: "C",
              text: "In the same direction proportional to the control surface it is attached to.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "A balance tab is linked to the fixed surface ahead of the control. When the pilot moves the main control surface, the tab is mechanically forced to move in the opposite direction. The airflow on the tab creates a force that helps move the main surface, reducing pilot effort.",
        },
        {
          id: 222,
          category: "Aircraft Controls",
          text: "If an aircraft is yawing to the left, where would you position the trim tab on the rudder?",
          options: [
            {
              letter: "A",
              text: "To the centre.",
            },
            {
              letter: "B",
              text: "To the left.",
            },
            {
              letter: "C",
              text: "To the right.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "If the aircraft is yawing left, it means the nose is going left. To correct this, the pilot needs to move the rudder to the right. The trim tab is positioned opposite the desired rudder deflection. To hold the rudder to the right, the trim tab on the rudder should be positioned to the left.",
        },
        {
          id: 223,
          category: "Aircraft Controls",
          text: "If an aircraft is flying with a left wing low, where would you move the left aileron trim tab?",
          options: [
            {
              letter: "A",
              text: "Down.",
            },
            {
              letter: "B",
              text: "Up.",
            },
            {
              letter: "C",
              text: "Moving the aileron trim tab will not correct the situation.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "A left wing low requires more lift on the left wing. This is achieved by moving the left aileron down. The trim tab moves opposite to the aileron. To hold the left aileron down, the trim tab on that aileron must be moved up.",
        },
        {
          id: 224,
          category: "Aerodynamics",
          text: "When a leading edge flap is fully extended, what is the slot in the wing for?",
          options: [
            {
              letter: "A",
              text: "To re-energise the boundary layer.",
            },
            {
              letter: "B",
              text: "To increase the lift.",
            },
            {
              letter: "C",
              text: "To allow the flap to retract into it when it retracts.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The slot created between the leading edge device (slat or flap) and the main wing acts as a duct. It directs high-energy air from the high-pressure area below the wing onto the upper surface boundary layer. This re-energizes the boundary layer, delaying separation and allowing flight at higher angles of attack.",
        },
        {
          id: 225,
          category: "Aircraft Controls",
          text: "With respect to differential aileron control, which of the following is true?",
          options: [
            {
              letter: "A",
              text: "The up going and down going ailerons both deflect to the same angle.",
            },
            {
              letter: "B",
              text: "The up going Aileron moves through a smaller angle than the down going aileron.",
            },
            {
              letter: "C",
              text: "The down going aileron moves through a smaller angle than the up going aileron.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Differential ailerons are designed to reduce adverse yaw. The aileron moving up deflects more than the aileron moving down. The increased drag on the upward-deflected aileron helps pull the nose into the turn, counteracting adverse yaw.",
        },
        {
          id: 226,
          category: "Aerodynamics",
          text: "The aeroplane fin is of symmetrical aerofoil section and will therefore provide a side-load",
          options: [
            {
              letter: "A",
              text: "only when the rudder is moved.",
            },
            {
              letter: "B",
              text: "if a suitable angle of attack develops due either yaw or rudder movement.",
            },
            {
              letter: "C",
              text: "only if a suitable angle of attack develops due to yaw.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "A symmetrical airfoil produces lift (or side-force) only when at an angle of attack. The fin can achieve this angle of attack either from the aircraft yawing (weathercock stability) or from the pilot deflecting the rudder.",
        },
        {
          id: 227,
          category: "Aircraft Controls",
          text: "An aircraft left wing is flying low. The aileron trimmer control to the left aileron trim tab in the cockpit would be",
          options: [
            {
              letter: "A",
              text: "moved up causing the left aileron to move up.",
            },
            {
              letter: "B",
              text: "moved up causing the left aileron to move down.",
            },
            {
              letter: "C",
              text: "moved down causing the left aileron to move down.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "To raise a low left wing, you need the left aileron to go down. The trim tab moves opposite to the aileron. To hold the aileron down, the tab must be up. Therefore, the cockpit trim control would be moved to command the tab up, which aerodynamically forces the aileron down.",
        },
        {
          id: 228,
          category: "Aircraft Controls",
          text: "An elevator tab moves down",
          options: [
            {
              letter: "A",
              text: "to make the nose go down.",
            },
            {
              letter: "B",
              text: "to counteract for the aircraft flying nose heavy.",
            },
            {
              letter: "C",
              text: "to counteract for the aircraft flying tail heavy.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "If the aircraft is tail-heavy, the nose wants to pitch up. To correct this, the pilot needs to apply a nose-down force, which is done by moving the elevator up. The trim tab moves opposite to the elevator. To hold the elevator up, the trim tab must move down.",
        },
        {
          id: 229,
          category: "Aerodynamics",
          text: "The stall margin is controlled by",
          options: [
            {
              letter: "A",
              text: "speed bug cursor.",
            },
            {
              letter: "B",
              text: "EPR limits.",
            },
            {
              letter: "C",
              text: "angle of attack and flap position.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Stall margin is the margin between the current angle of attack and the critical angle of attack. Flap position changes the critical AoA. Therefore, AoA and flap position are the primary factors in determining how close the aircraft is to stalling.",
        },
        {
          id: 230,
          category: "Aircraft Controls",
          text: "Other than spoilers, where are speed brakes located?",
          options: [
            {
              letter: "A",
              text: "Under the Fuselage.",
            },
            {
              letter: "B",
              text: "Either side of the Fuselage.",
            },
            {
              letter: "C",
              text: "On the wing.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "While spoilers are on the wing and can act as speed brakes, dedicated speed brakes are often large panels that extend from the sides of the rear fuselage to increase drag without significantly affecting lift.",
        },
        {
          id: 231,
          category: "Aerodynamics",
          text: "With a trailing edge flap being lowered, due to rising gusts, what will happen to the angle of attack?",
          options: [
            {
              letter: "A",
              text: "Tend to decrease.",
            },
            {
              letter: "B",
              text: "Stay the same.",
            },
            {
              letter: "C",
              text: "Tend to increase.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Lowering flaps increases the camber and downwash behind the wing. This downwash increases the angle of attack on the tailplane, which increases its downforce. This increased downforce tends to pitch the nose down, which would reduce the aircraft's angle of attack.",
        },
        {
          id: 232,
          category: "Aircraft Controls",
          text: "A device used do dump lift from an aircraft is",
          options: [
            {
              letter: "A",
              text: "leading edge flaps.",
            },
            {
              letter: "B",
              text: "trailing edge flaps.",
            },
            {
              letter: "C",
              text: "spoiler.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Spoilers, when deployed, disrupt the smooth airflow over the wing, drastically reducing lift (dumping lift). They are used on landing to transfer weight onto the wheels for better braking.",
        },
        {
          id: 233,
          category: "Aerodynamics",
          text: "The purpose of a slot in a wing is to",
          options: [
            {
              letter: "A",
              text: "provide housing for the slat.",
            },
            {
              letter: "B",
              text: "speed up the airflow and increase lift.",
            },
            {
              letter: "C",
              text: "act as venturi, accelerate the air and re-energise boundary layer.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "A slot, typically created by a slat, acts as a venturi. It accelerates the air passing through it. This high-energy air is then directed onto the upper surface boundary layer, giving it more energy to resist separation at high angles of attack.",
        },
        {
          id: 234,
          category: "Aerodynamics",
          text: "Large flap deployment",
          options: [
            {
              letter: "A",
              text: "causes increased span wise flow towards tips on wing upper surface.",
            },
            {
              letter: "B",
              text: "causes increased span wise flow towards tips on wing lower surface.",
            },
            {
              letter: "C",
              text: "has no effect on span wise flow.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Flap deployment greatly increases the pressure difference between the upper and lower surfaces, especially inboard. This strengthens the spanwise pressure gradient, causing the boundary layer on the upper surface to flow more strongly towards the tips.",
        },
        {
          id: 235,
          category: "Aerodynamics",
          text: "Which part of the wing of a swept-wing aircraft stalls first?",
          options: [
            {
              letter: "A",
              text: "Tip stalls first.",
            },
            {
              letter: "B",
              text: "Both stall together.",
            },
            {
              letter: "C",
              text: "Root stalls first.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "On a swept-back wing, the tips are aerodynamically aft and tend to have higher local angles of attack due to spanwise flow. They typically stall before the root, which can lead to a nose-up pitching moment.",
        },
        {
          id: 236,
          category: "Stability",
          text: "During flight, an aircraft is yawing to the right. The aircraft would have a tendency to fly",
          options: [
            {
              letter: "A",
              text: "right wing low.",
            },
            {
              letter: "B",
              text: "left wing low.",
            },
            {
              letter: "C",
              text: "nose up.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Yawing to the right means the nose is pointing right. The left wing is moving faster through the air than the right wing (due to its longer radius of turn). This creates more lift on the left wing, causing the aircraft to bank (roll) to the right (right wing low). This is known as roll due to yaw.",
        },
        {
          id: 237,
          category: "Aircraft Design",
          text: "In the reversed camber horizontal stabilizer",
          options: [
            {
              letter: "A",
              text: "there is an increased tail plane up-force.",
            },
            {
              letter: "B",
              text: "the elevator causes tail down movement i.e. increased tail plane down force.",
            },
            {
              letter: "C",
              text: "there is an increased tailplane down-force.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "A reversed camber (or supercritical) horizontal stabilizer is designed to generate negative lift (downforce) more efficiently. Its shape is essentially an inverted airfoil, optimized to produce the required downforce for longitudinal stability with less drag.",
        },
        {
          id: 238,
          category: "Aerodynamics",
          text: "When the trailing edge flap is extended",
          options: [
            {
              letter: "A",
              text: "CP moves rearward.",
            },
            {
              letter: "B",
              text: "the CP moves forward but the CG does not change.",
            },
            {
              letter: "C",
              text: "the CP moves forward and the pitching moment changes to nose up.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Extending trailing edge flaps increases the camber and lift, especially on the rear portion of the airfoil. This causes the center of pressure to move forward. The CG remains unchanged, but the forward shift of the CP creates a nose-down pitching moment (not nose-up).",
        },
        {
          id: 239,
          category: "Aerodynamics",
          text: "With a drop in ambient temperature, an aircraft service ceiling will",
          options: [
            {
              letter: "A",
              text: "rise.",
            },
            {
              letter: "B",
              text: "not be affected.",
            },
            {
              letter: "C",
              text: "lower.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Service ceiling is the altitude where the maximum rate of climb is 100 ft/min. Colder air is denser. A denser atmosphere provides more oxygen for engines and more lift for the wings, allowing the aircraft to climb to a higher altitude before its climb rate drops to 100 ft/min.",
        },
        {
          id: 240,
          category: "Aircraft Controls",
          text: "Servo tabs",
          options: [
            {
              letter: "A",
              text: "enable the pilot to bring the control surface back to neutral.",
            },
            {
              letter: "B",
              text: "move in such a way as to help move the control surface.",
            },
            {
              letter: "C",
              text: "provide artificial feel.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "A servo tab is a type of balance tab that is directly connected to the pilot's controls. When the pilot moves the control, the tab moves in the opposite direction. The aerodynamic force on the tab then helps to move the main control surface, reducing pilot effort.",
        },
        {
          id: 241,
          category: "Aircraft Controls",
          text: "Spring Tabs",
          options: [
            {
              letter: "A",
              text: "provide artificial feel.",
            },
            {
              letter: "B",
              text: "enable the pilot to bring the control surface back to neutral.",
            },
            {
              letter: "C",
              text: "move in such a way as to help move the control surface.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "A spring tab is similar to a servo tab, but it includes a spring in the linkage. At low speeds, when control forces are light, the spring doesn't compress, and the tab doesn't assist. As speed and control forces increase, the spring compresses, allowing the tab to move and provide aerodynamic assistance. It helps move the control surface at high speeds.",
        },
        {
          id: 242,
          category: "Aerodynamics",
          text: "Extending a leading edge slat will have what effect on the angle of attack of a wing?",
          options: [
            {
              letter: "A",
              text: "Increase the angle of attack.",
            },
            {
              letter: "B",
              text: "Decrease the angle of attack.",
            },
            {
              letter: "C",
              text: "No effect on angle of attack.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The angle of attack is defined by the aircraft's flight path and attitude relative to the airflow. Extending a slat is a change to the wing's geometry. It changes the critical angle of attack (allowing a higher AoA before stall), but it does not, in itself, change the aircraft's current angle of attack.",
        },
        {
          id: 243,
          category: "Aerodynamics",
          text: "To ensure that a wing stalls at the root first, stall wedges are",
          options: [
            {
              letter: "A",
              text: "installed on the wing leading edge at the wing root.",
            },
            {
              letter: "B",
              text: "installed on the wing leading edge at the wing tip.",
            },
            {
              letter: "C",
              text: "installed at the wing trailing edge at the wing root.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Stall wedges (or strips) are small, angled pieces of metal attached to the leading edge of the wing root. They intentionally disrupt the airflow at the root at high angles of attack, ensuring the root stalls before the tip, providing buffet warning and maintaining aileron effectiveness.",
        },
        {
          id: 244,
          category: "Aircraft Design",
          text: "Krueger flaps make up part of the",
          options: [
            {
              letter: "A",
              text: "wing lower surface leading edge.",
            },
            {
              letter: "B",
              text: "wing lower surface trailing edge.",
            },
            {
              letter: "C",
              text: "wing upper surface leading edge.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Krueger flaps are a type of leading-edge high-lift device. They are hinged at the front of the wing's lower surface and, when deployed, they swing down and forward from under the leading edge. This increases the camber and angle of attack capability of the wing.",
        },
        {
          id: 245,
          category: "Aircraft Controls",
          text: "In a turn, wing spoilers may be deployed",
          options: [
            {
              letter: "A",
              text: "to assist the up going aileron.",
            },
            {
              letter: "B",
              text: "in unison with both the up going and down going ailerons.",
            },
            {
              letter: "C",
              text: "to act as an airbrake, interacting with the ailerons.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "On many large aircraft, spoilers are used in conjunction with ailerons for roll control. To initiate a roll to the right, the right aileron goes up and the right spoilers may also deploy. This kills lift on the right wing, assisting the up-going aileron and providing additional roll authority.",
        },
        {
          id: 246,
          category: "Stability",
          text: "Dutch roll is movement in",
          options: [
            {
              letter: "A",
              text: "pitch and roll.",
            },
            {
              letter: "B",
              text: "yaw and roll.",
            },
            {
              letter: "C",
              text: "yaw and pitch.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Dutch roll is a coupled oscillation in yaw and roll. The aircraft yaws, which causes a roll due to the speed difference of the wings, and then the dihedral effect causes a roll that leads to a yaw in the opposite direction, creating an oscillatory motion.",
        },
        {
          id: 247,
          category: "Aircraft Controls",
          text: "What is the main purpose of a frise aileron?",
          options: [
            {
              letter: "A",
              text: "Help pilot overcome aerodynamic loads.",
            },
            {
              letter: "B",
              text: "Decrease drag on the up going wing.",
            },
            {
              letter: "C",
              text: "Increase drag on the up going wing.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "A Frise aileron is designed with a shaped leading edge that protrudes into the airflow when the aileron is deflected upward. This creates significant drag on the upgoing wing, which helps to counteract adverse yaw by pulling the nose into the turn.",
        },
        {
          id: 248,
          category: "Aircraft Controls",
          text: "Flap asymmetry causes the aircraft to",
          options: [
            {
              letter: "A",
              text: "nose down.",
            },
            {
              letter: "B",
              text: "go one wing down.",
            },
            {
              letter: "C",
              text: "nose up.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Flap asymmetry means one flap is extended more than the other. The wing with the greater flap extension will produce more lift and more drag. The difference in lift will cause that wing to rise, and the difference in drag will cause the aircraft to yaw towards the wing with less flap.",
        },
        {
          id: 249,
          category: "Stability",
          text: "If an aircraft moves in yaw, what axis is it moving about?",
          options: [
            {
              letter: "A",
              text: "Longitudinal.",
            },
            {
              letter: "B",
              text: "Lateral.",
            },
            {
              letter: "C",
              text: "Normal.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Yaw is rotation about the normal (or vertical) axis, which runs vertically through the center of gravity.",
        },
        {
          id: 250,
          category: "Stability",
          text: "If an aircraft is aerodynamically stable",
          options: [
            {
              letter: "A",
              text: "aircraft returns to trimmed attitude.",
            },
            {
              letter: "B",
              text: "CofP moves back.",
            },
            {
              letter: "C",
              text: "aircraft becomes too sensitive.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Static stability is the initial tendency of an aircraft to return to its original trimmed state after being disturbed. A positive static stability means the aircraft will naturally try to return to its equilibrium attitude.",
        },
        {
          id: 251,
          category: "Aircraft Controls",
          text: "What are ground spoilers used for?",
          options: [
            {
              letter: "A",
              text: "To assist the aircraft coming to a stop.",
            },
            {
              letter: "B",
              text: "To slow the aircraft.",
            },
            {
              letter: "C",
              text: "To dump lift.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The primary purpose of ground spoilers is to 'dump' or destroy the lift being generated by the wings after landing. This transfers the weight of the aircraft onto the wheels, allowing the brakes to work more effectively without skidding. Slowing the aircraft is a secondary effect.",
        },
        {
          id: 252,
          category: "Aircraft Controls",
          text: "Mass balance weights are used to",
          options: [
            {
              letter: "A",
              text: "balance the trailing edge of flying control surfaces.",
            },
            {
              letter: "B",
              text: "counteract flutter on control surfaces.",
            },
            {
              letter: "C",
              text: "balance the tabs.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Mass balance involves placing weights forward of the control surface hinge line. This ensures that the center of gravity of the control surface is at the hinge line. This prevents flutter, a dangerous, high-frequency oscillation that can be caused by aerodynamic forces acting on an unbalanced surface.",
        },
        {
          id: 253,
          category: "Aerodynamics",
          text: "What is a slot used for?",
          options: [
            {
              letter: "A",
              text: "Increased angle of attack during approach.",
            },
            {
              letter: "B",
              text: "Increase the speed of the airflow.",
            },
            {
              letter: "C",
              text: "To reinforce the boundary layer.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "A slot, typically between a slat and the wing, directs high-energy air onto the upper surface. This re-energizes the boundary layer, reinforcing it against separation and allowing the wing to operate at higher angles of attack without stalling.",
        },
        {
          id: 254,
          category: "Aerodynamics",
          text: "Angle of Attack is the angle between chord",
          options: [
            {
              letter: "A",
              text: "relative air flow.",
            },
            {
              letter: "B",
              text: "horizontal axis.",
            },
            {
              letter: "C",
              text: "tip path plane.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Angle of Attack (AoA) is defined as the angle between the chord line of the airfoil and the direction of the relative airflow (or relative wind).",
        },
        {
          id: 255,
          category: "Aerodynamics",
          text: "A high lift device is used for",
          options: [
            {
              letter: "A",
              text: "take-off only.",
            },
            {
              letter: "B",
              text: "take-off and landing.",
            },
            {
              letter: "C",
              text: "landing only.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "High lift devices, such as flaps and slats, are used during both take-off and landing. For take-off, they are set to an intermediate position to increase lift and shorten the takeoff run. For landing, they are set to a fully extended position to maximize lift and allow for a slow, safe approach speed.",
        },
        {
          id: 256,
          category: "Aircraft Controls",
          text: "How is a spoiler interconnected to other flight control systems?",
          options: [
            {
              letter: "A",
              text: "Spoiler to elevator.",
            },
            {
              letter: "B",
              text: "Spoiler to aileron.",
            },
            {
              letter: "C",
              text: "Spoiler to flap.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Spoilers are often interconnected with the aileron control system. When the pilot moves the control wheel for a roll input, the spoilers on the downgoing wing deploy to assist the ailerons.",
        },
        {
          id: 257,
          category: "Aircraft Controls",
          text: "What is aileron droop?",
          options: [
            {
              letter: "A",
              text: "The droop of ailerons with no hydraulics on.",
            },
            {
              letter: "B",
              text: "The leading edge of both ailerons presented to the airflow.",
            },
            {
              letter: "C",
              text: "One aileron lowered.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Aileron droop refers to a configuration where both ailerons are symmetrically lowered by a small amount, usually to act as flaps and increase the wing's camber for better low-speed performance. In some contexts, it can also refer to the ailerons hanging down when hydraulic pressure is lost.",
        },
        {
          id: 258,
          category: "ISA/Atmosphere",
          text: "Earths atmosphere is",
          options: [
            {
              letter: "A",
              text: "3/5 oxygen, 2/5 nitrogen.",
            },
            {
              letter: "B",
              text: "4/5 oxygen, 1/5 nitrogen.",
            },
            {
              letter: "C",
              text: "1/5 oxygen, 4/5 nitrogen.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The Earth's atmosphere is approximately 78% nitrogen and 21% oxygen. 78% is roughly 4/5, and 21% is roughly 1/5.",
        },
        {
          id: 259,
          category: "Aircraft Controls",
          text: "An anti-balance tab is used",
          options: [
            {
              letter: "A",
              text: "to relieve stick loads.",
            },
            {
              letter: "B",
              text: "for trimming the aircraft.",
            },
            {
              letter: "C",
              text: "to give more feel to the controls.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "An anti-balance (or anti-servo) tab moves in the same direction as the main control surface. This adds to the control surface's effectiveness and increases the force required to move it, providing the pilot with more 'feel' or artificial feedback, especially on aircraft with fully powered controls.",
        },
        {
          id: 260,
          category: "Stability",
          text: "The fin helps to give",
          options: [
            {
              letter: "A",
              text: "directional stability about the normal axis.",
            },
            {
              letter: "B",
              text: "directional stability about the longitudinal axis.",
            },
            {
              letter: "C",
              text: "longitudinal stability about the normal axis.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The fin (vertical stabilizer) is the primary surface that provides directional stability. Directional stability is stability about the normal (vertical) axis, meaning it resists yawing motions.",
        },
        {
          id: 261,
          category: "Stability",
          text: "If an aircraft moves in roll, it is moving about the",
          options: [
            {
              letter: "A",
              text: "longitudinal axis.",
            },
            {
              letter: "B",
              text: "normal axis.",
            },
            {
              letter: "C",
              text: "lateral axis.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Roll is the rotation of an aircraft about its longitudinal axis, which runs from the nose to the tail.",
        },
        {
          id: 262,
          category: "Aerodynamics",
          text: "What effect does lowering the flaps for take-off have?",
          options: [
            {
              letter: "A",
              text: "Increases lift & reduces drag.",
            },
            {
              letter: "B",
              text: "Increases lift and drag.",
            },
            {
              letter: "C",
              text: "Increase lift only.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Lowering flaps increases both lift and drag. For takeoff, a small flap setting is used to get a significant increase in lift with a relatively small increase in drag, allowing the aircraft to become airborne at a lower speed.",
        },
        {
          id: 263,
          category: "Aerodynamics",
          text: "What effect does lowering flaps for takeoff have?",
          options: [
            {
              letter: "A",
              text: "Reduces takeoff speeds only.",
            },
            {
              letter: "B",
              text: "Reduces landing speeds only.",
            },
            {
              letter: "C",
              text: "Reduces takeoff and landing speeds.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Flaps for takeoff are specifically set to reduce the takeoff speed, thereby shortening the takeoff ground roll. Landing speeds are reduced by the full landing flap setting.",
        },
        {
          id: 264,
          category: "Aerodynamics",
          text: "When the flaps are lowered",
          options: [
            {
              letter: "A",
              text: "the lift vector moves rearward.",
            },
            {
              letter: "B",
              text: "there is no effect on the lift vector.",
            },
            {
              letter: "C",
              text: "the lift vector moves forward.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Lowering flaps increases the lift generated by the rear portion of the airfoil. This effectively shifts the center of pressure (the point where the lift vector acts) rearward along the chord.",
        },
        {
          id: 265,
          category: "Aerodynamics",
          text: "At take-off, if the flaps are lowered there is a",
          options: [
            {
              letter: "A",
              text: "large increase in lift and drag.",
            },
            {
              letter: "B",
              text: "large increase in lift and small increase in drag.",
            },
            {
              letter: "C",
              text: "small increase in lift and drag.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Take-off flap settings are designed to provide a substantial increase in lift (to get airborne at a lower speed) while keeping the drag increase relatively small to allow for good acceleration.",
        },
        {
          id: 266,
          category: "Aircraft Controls",
          text: "Wing spoilers can be used",
          options: [
            {
              letter: "A",
              text: "as ground spoilers on landing.",
            },
            {
              letter: "B",
              text: "to assist the respective down going aileron in a turn.",
            },
            {
              letter: "C",
              text: "to assist the elevators.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Wing spoilers have multiple functions. They can be used symmetrically in flight as speed brakes, asymmetrically to assist ailerons in roll control, and symmetrically on the ground as lift dumpers to improve braking effectiveness.",
        },
        {
          id: 267,
          category: "Aircraft Controls",
          text: "Differential aileron control will",
          options: [
            {
              letter: "A",
              text: "cause a nose down moment.",
            },
            {
              letter: "B",
              text: "prevent yawing in conjunction with rudder input.",
            },
            {
              letter: "C",
              text: "cause a nose up moment.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The primary purpose of differential ailerons is to reduce or eliminate adverse yaw. By having the up-going aileron deflect more than the down-going one, the extra drag on the up-going wing counteracts the tendency for the aircraft to yaw opposite the direction of the turn.",
        },
        {
          id: 268,
          category: "Stability",
          text: "Dutch Roll affects",
          options: [
            {
              letter: "A",
              text: "pitch and yaw simultaneously.",
            },
            {
              letter: "B",
              text: "yaw and roll simultaneously.",
            },
            {
              letter: "C",
              text: "pitch and roll simultaneously.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Dutch roll is a coupled oscillatory motion involving both yaw and roll.",
        },
        {
          id: 269,
          category: "Aircraft Controls",
          text: "Which of the following are primary control surfaces?",
          options: [
            {
              letter: "A",
              text: "Elevators, ailerons, rudder.",
            },
            {
              letter: "B",
              text: "Roll spoilers, elevators, tabs.",
            },
            {
              letter: "C",
              text: "Elevators, roll spoilers, tabs.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The primary flight controls are the surfaces that control the aircraft's attitude about its three axes: elevators (pitch), ailerons (roll), and rudder (yaw).",
        },
        {
          id: 270,
          category: "Aircraft Controls",
          text: "An anti-servo tab",
          options: [
            {
              letter: "A",
              text: "assists the pilot to move the controls back to neutral.",
            },
            {
              letter: "B",
              text: "moves in the opposite direction to the control surface to assist the pilot.",
            },
            {
              letter: "C",
              text: "moves in the same direction as the control surface to assist the pilot.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "An anti-servo tab moves in the same direction as the main control surface. This increases the control surface's effectiveness and also increases the force needed to move it, providing the pilot with artificial feel. It is the opposite of a servo tab.",
        },
        {
          id: 271,
          category: "Aerodynamics",
          text: "Slats",
          options: [
            {
              letter: "A",
              text: "keep the boundary layer from separating for longer.",
            },
            {
              letter: "B",
              text: "increase the overall surface area and lift effect of wing.",
            },
            {
              letter: "C",
              text: "act as an air brake.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Slats are leading-edge devices that create a slot. High-energy air from below the wing is directed through the slot onto the upper surface boundary layer, re-energizing it and delaying flow separation. This allows the wing to operate at higher angles of attack before stalling.",
        },
        {
          id: 272,
          category: "Aerodynamics",
          text: "Due to the change of lift forces resulting from the extension of flaps in flight",
          options: [
            {
              letter: "A",
              text: "nose should be lowered, reducing AOA.",
            },
            {
              letter: "B",
              text: "nose should be raised, increasing AOA.",
            },
            {
              letter: "C",
              text: "nose should remain in the same position, maintaining same AOA.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Extending flaps increases lift, causing the aircraft to want to climb. To maintain level flight at a constant speed, the pilot must lower the nose to reduce the angle of attack and compensate for the increased lift.",
        },
        {
          id: 273,
          category: "Aircraft Controls",
          text: "Flight spoilers",
          options: [
            {
              letter: "A",
              text: "can be deployed on the down going wing in a turn to increase lift on that wing.",
            },
            {
              letter: "B",
              text: "can be used to decrease lift to allow controlled decent without reduction of airspeed.",
            },
            {
              letter: "C",
              text: "can be used with differential ailerons to reduce adverse yaw in a turn.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Flight spoilers can be deployed symmetrically to act as speed brakes. By disrupting lift and increasing drag, they allow the aircraft to descend rapidly without an increase in airspeed.",
        },
        {
          id: 274,
          category: "Aircraft Controls",
          text: "If the aircraft is flying nose heavy, which direction would you move the elevator trim tab?",
          options: [
            {
              letter: "A",
              text: "Up to move elevator down.",
            },
            {
              letter: "B",
              text: "Up to move elevator up.",
            },
            {
              letter: "C",
              text: "Down to move elevator up.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Nose heavy means the pilot has to hold back pressure (elevator up) to keep the nose up. To trim this out, the trim tab needs to hold the elevator in the up position. Trim tabs move opposite the control surface. To hold the elevator up, the trim tab must be moved down.",
        },
        {
          id: 275,
          category: "Aerodynamics",
          text: "Wing tip vortices are strongest when",
          options: [
            {
              letter: "A",
              text: "flying high speed straight and level flight.",
            },
            {
              letter: "B",
              text: "flying into a headwind.",
            },
            {
              letter: "C",
              text: "flying slowly at high angles of attack.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Vortex strength is directly related to the amount of lift being generated. At slow speeds and high angles of attack (e.g., during takeoff and landing), the wing is producing a large amount of lift, which results in the strongest wingtip vortices.",
        },
        {
          id: 276,
          category: "Aircraft Controls",
          text: "Aerodynamic balance",
          options: [
            {
              letter: "A",
              text: "will reduce aerodynamic loading.",
            },
            {
              letter: "B",
              text: "will cause CP to move forward of hinge and cause overbalance.",
            },
            {
              letter: "C",
              text: "will cause CP to move towards the trailing edge and cause instability.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Aerodynamic balance refers to methods (like horn balances) that position part of the control surface ahead of the hinge line. This creates a force that assists the pilot. If too much area is ahead of the hinge, the center of pressure can move forward of the hinge, causing the control surface to snap to its stop (overbalance), a dangerous condition.",
        },
        {
          id: 277,
          category: "Aircraft Controls",
          text: "A balance tab",
          options: [
            {
              letter: "A",
              text: "effectively increases the area of the control surface.",
            },
            {
              letter: "B",
              text: "assists the pilot to move the controls.",
            },
            {
              letter: "C",
              text: "is used to trim the appropriate axis of the aircraft.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "A balance tab is a small surface on the trailing edge of a primary control. It is linked to move in the opposite direction of the main surface, creating an aerodynamic force that helps move the main control, thereby reducing pilot effort.",
        },
        {
          id: 278,
          category: "Aircraft Controls",
          text: "Elevons combine the functions of both",
          options: [
            {
              letter: "A",
              text: "rudder and elevator.",
            },
            {
              letter: "B",
              text: "elevator and aileron.",
            },
            {
              letter: "C",
              text: "rudder and aileron.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Elevons, found on delta-wing aircraft, combine the functions of elevators (pitch) and ailerons (roll). When moved together, they control pitch; when moved differentially, they control roll.",
        },
        {
          id: 279,
          category: "Aircraft Controls",
          text: "Flutter can be reduced by using",
          options: [
            {
              letter: "A",
              text: "a horn balance.",
            },
            {
              letter: "B",
              text: "mass balancing.",
            },
            {
              letter: "C",
              text: "servo tabs.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Flutter is a dangerous, high-frequency oscillation caused by the interaction of aerodynamic forces and the structural elasticity of the control surface. Mass balancing adds weights forward of the hinge line to ensure the control surface's center of gravity is at the hinge line, preventing it from acting like a pendulum and initiating flutter.",
        },
        {
          id: 280,
          category: "Aircraft Controls",
          text: "An elevator provides control about the",
          options: [
            {
              letter: "A",
              text: "longitudinal axis.",
            },
            {
              letter: "B",
              text: "lateral axis.",
            },
            {
              letter: "C",
              text: "horizontal stabilizer.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The elevator controls pitch, which is rotation about the lateral axis (the axis that runs from wingtip to wingtip through the center of gravity).",
        },
        {
          id: 281,
          category: "Aircraft Controls",
          text: "The outboard ailerons on some large aircraft",
          options: [
            {
              letter: "A",
              text: "are isolated at high speeds.",
            },
            {
              letter: "B",
              text: "are isolated to improve sensitivity.",
            },
            {
              letter: "C",
              text: "are isolated at low speeds.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "On many large transport aircraft, the outboard ailerons are locked out at high speeds. This is because at high speeds, the ailerons could cause excessive structural loads or wing twist. Inboard ailerons or spoilers are used for roll control in this regime.",
        },
        {
          id: 282,
          category: "Aerodynamics",
          text: "Which wing increases drag when the ailerons are moved?",
          options: [
            {
              letter: "A",
              text: "Both wings increase drag but the wing with the down-going aileron increases more.",
            },
            {
              letter: "B",
              text: "Both wings have an equal increase in drag.",
            },
            {
              letter: "C",
              text: "Both wings increase drag but the wing with the up-going aileron increases more.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "When ailerons are deflected, the up-going aileron (on the wing that goes down) creates a significant amount of drag due to its downward deflection increasing lift and induced drag, and also possibly creating form drag. This difference in drag is the primary cause of adverse yaw.",
        },
        {
          id: 283,
          category: "Aircraft Design",
          text: "Which flap will increase wing area and camber?",
          options: [
            {
              letter: "A",
              text: "Slot.",
            },
            {
              letter: "B",
              text: "Split.",
            },
            {
              letter: "C",
              text: "Fowler.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "A Fowler flap is a type of trailing edge flap that, when extended, moves rearwards on tracks before hinging downwards. This rearward movement increases the effective wing area, while the downward movement increases the camber, providing a significant increase in lift coefficient.",
        },
        {
          id: 284,
          category: "Aircraft Design",
          text: "Wing loading of an aircraft",
          options: [
            {
              letter: "A",
              text: "varies with dynamic loading due to air currents.",
            },
            {
              letter: "B",
              text: "is independent of altitude.",
            },
            {
              letter: "C",
              text: "decreases with density.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Wing loading is a fixed design parameter defined as the aircraft's weight divided by its wing area. It does not change with altitude, density, or dynamic loading, although the effects of wing loading on performance (like stall speed) do vary with density.",
        },
        {
          id: 285,
          category: "Aerodynamics",
          text: "An automatic slat will lift by itself when the angle of attack is",
          options: [
            {
              letter: "A",
              text: "high.",
            },
            {
              letter: "B",
              text: "high or low.",
            },
            {
              letter: "C",
              text: "low.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Automatic (or aerodynamic) slats are not directly controlled by the pilot. They are held closed by air pressure at low angles of attack. As angle of attack increases, the low-pressure area on the upper surface of the wing moves forward, creating a pressure differential that sucks the slat forward and open.",
        },
        {
          id: 286,
          category: "Aircraft Controls",
          text: "On aircraft fitted with spoilers for lateral control, roll to the right is caused by",
          options: [
            {
              letter: "A",
              text: "left spoilers extending, right spoilers remaining retracted.",
            },
            {
              letter: "B",
              text: "right spoilers extending, left spoilers remaining retracted.",
            },
            {
              letter: "C",
              text: "left and right spoilers extending.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "To roll to the right, the pilot wants to decrease lift on the left wing and/or increase lift on the right wing. Spoilers destroy lift. Therefore, extending the spoilers on the left wing will cause that wing to drop, rolling the aircraft to the right.",
        },
        {
          id: 287,
          category: "Aerodynamics",
          text: "A split flap increases lift by increasing",
          options: [
            {
              letter: "A",
              text: "the angle of attachment of the lower hinged portion.",
            },
            {
              letter: "B",
              text: "the surface area.",
            },
            {
              letter: "C",
              text: "the camber of the top surface.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "A split flap is hinged on the lower surface of the wing. When deployed, it hinges downward, increasing the camber of the lower surface and creating a low-pressure area behind it, which increases lift. It does not significantly increase wing area or change the camber of the top surface.",
        },
        {
          id: 288,
          category: "Aerodynamics",
          text: "When the trailing edge flaps are lowered, the aircraft will",
          options: [
            {
              letter: "A",
              text: "pitch nose up.",
            },
            {
              letter: "B",
              text: "pitch nose down.",
            },
            {
              letter: "C",
              text: "sink.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Lowering flaps increases the camber and lift of the wing. This increase in lift, acting behind the center of gravity, creates a nose-down pitching moment? Let's re-evaluate: The increase in lift occurs mainly on the rear of the wing, which is usually aft of the CG. This additional lift behind the CG creates a nose-down moment. However, on many aircraft, the downwash from the flaps on the tailplane can create a powerful nose-up moment that overcomes the wing's nose-down moment. The most common initial reaction is a nose-up pitch, requiring the pilot to push forward to maintain altitude. The question's correct answer is likely based on the most typical immediate effect.",
        },
        {
          id: 289,
          category: "Aircraft Controls",
          text: "In aileron control",
          options: [
            {
              letter: "A",
              text: "the up going aileron moves further than down going aileron.",
            },
            {
              letter: "B",
              text: "the down going aileron moves further than up going aileron.",
            },
            {
              letter: "C",
              text: "it is assisted by the rudder.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "This describes differential ailerons. The up-going aileron deflects more than the down-going one. This helps to reduce adverse yaw by creating more drag on the down-going wing.",
        },
        {
          id: 290,
          category: "Aircraft Controls",
          text: "The aircraft is controlled about the lateral axis by the",
          options: [
            {
              letter: "A",
              text: "ailerons.",
            },
            {
              letter: "B",
              text: "elevator.",
            },
            {
              letter: "C",
              text: "rudder.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The lateral axis runs from wingtip to wingtip. Movement about this axis is pitch, which is controlled by the elevator.",
        },
        {
          id: 291,
          category: "Aircraft Controls",
          text: "The aircraft is controlled about the normal axis by the",
          options: [
            {
              letter: "A",
              text: "ailerons.",
            },
            {
              letter: "B",
              text: "elevator.",
            },
            {
              letter: "C",
              text: "rudder.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The normal (or vertical) axis runs vertically through the CG. Movement about this axis is yaw, which is controlled by the rudder.",
        },
        {
          id: 292,
          category: "Stability",
          text: "Dutch roll is",
          options: [
            {
              letter: "A",
              text: "a combined yawing and rolling motion.",
            },
            {
              letter: "B",
              text: "primarily a pitching instability.",
            },
            {
              letter: "C",
              text: "a type of slow roll.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Dutch roll is a coupled lateral-directional oscillation involving both yaw and roll.",
        },
        {
          id: 293,
          category: "Aircraft Controls",
          text: "The aircraft is controlled about the longitudinal axis by the",
          options: [
            {
              letter: "A",
              text: "ailerons.",
            },
            {
              letter: "B",
              text: "elevator.",
            },
            {
              letter: "C",
              text: "rudder.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The longitudinal axis runs from nose to tail. Movement about this axis is roll, which is controlled by the ailerons.",
        },
        {
          id: 294,
          category: "Aircraft Controls",
          text: "Ruddervators when moved, will move",
          options: [
            {
              letter: "A",
              text: "opposite to each other only.",
            },
            {
              letter: "B",
              text: "together only.",
            },
            {
              letter: "C",
              text: "either opposite each other or together, depending on the selection.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "On a V-tail aircraft, the ruddervators are controlled by a mixer unit. When the pilot pulls back on the yoke, both ruddervators move up together for pitch. When the pilot pushes the rudder pedals, they move opposite each other for yaw.",
        },
        {
          id: 295,
          category: "Stability",
          text: "As a consequence of the C of G being close to its aft limit",
          options: [
            {
              letter: "A",
              text: "the stick forces will be high in fore and aft pitch, due to the high longitudinal stability.",
            },
            {
              letter: "B",
              text: "the stick forces to manoeuvre longitudinally will be low due to the low stability.",
            },
            {
              letter: "C",
              text: "the stick forces when pitching the nose down will be very high.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "An aft CG reduces the aircraft's longitudinal stability. An aircraft with lower stability requires smaller control forces to maneuver, making it more responsive. This is why aft CG limits are critical; too far aft can lead to instability and very light, dangerous control forces.",
        },
        {
          id: 296,
          category: "Atmospheric Conditions",
          text: "What is the term used for the amount of water in the atmosphere?",
          options: [
            {
              letter: "A",
              text: "Relative humidity.",
            },
            {
              letter: "B",
              text: "Absolute humidity.",
            },
            {
              letter: "C",
              text: "Dew point.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Absolute humidity is the measure of the actual amount of water vapor present in the air, usually expressed as mass per unit volume.",
        },
        {
          id: 297,
          category: "Aircraft Controls",
          text: "An anti-balance tab is moved",
          options: [
            {
              letter: "A",
              text: "via a fixed linkage.",
            },
            {
              letter: "B",
              text: "hydraulically.",
            },
            {
              letter: "C",
              text: "when the C.G. changes.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "An anti-balance (or anti-servo) tab is typically connected via a fixed mechanical linkage so that it moves in the same direction as the main control surface. This provides artificial feel. It is not typically an on-demand trimming device like a trim tab.",
        },
        {
          id: 298,
          category: "Aircraft Controls",
          text: "A servo tab is operated",
          options: [
            {
              letter: "A",
              text: "automatically, and moves in the same direction as the main control surfaces.",
            },
            {
              letter: "B",
              text: "directly by the pilot to produce forces which in turn move the main control surfaces.",
            },
            {
              letter: "C",
              text: "by a trim wheel and moves in the opposite direction to the main control sufraces when moved.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "A servo tab is directly connected to the pilot's controls. The pilot's input moves the tab, and the aerodynamic force on the tab then moves the main control surface. The pilot doesn't directly move the main surface; they move the tab, which then flies the main surface into position.",
        },
        {
          id: 299,
          category: "Aircraft Controls",
          text: "On an aircraft with an all-moving tailplane, pitch up is caused by",
          options: [
            {
              letter: "A",
              text: "decreasing tailplane incidence.",
            },
            {
              letter: "B",
              text: "up movement of the elevator trim tab.",
            },
            {
              letter: "C",
              text: "increasing tailplane incidence.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "An all-moving tailplane (or stabilator) pivots as a single unit. To pitch the nose up, the leading edge of the tailplane is moved up, which increases the tailplane's angle of attack, creating more downward force on the tail, which lifts the nose.",
        },
        {
          id: 300,
          category: "Aircraft Maintenance",
          text: "When checking full range of control surface movement, they must be positioned by",
          options: [
            {
              letter: "A",
              text: "moving them by hand directly until against the primary stops.",
            },
            {
              letter: "B",
              text: "moving them by hand directly until against the secondary stops.",
            },
            {
              letter: "C",
              text: "operating the control cabin controls until the system is against the primary stops.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Control surface travel limits are checked by operating the cockpit controls fully. The control system will have its own internal stops (primary stops) that limit the movement of the cockpit controls. The control surfaces themselves may also have stops, but the full range is determined by the cockpit input.",
        },
        {
          id: 301,
          category: "Aircraft Controls",
          text: "An excess of aerodynamic balance would move the control surface centre of pressure",
          options: [
            {
              letter: "A",
              text: "rearwards, resulting in too much assistance.",
            },
            {
              letter: "B",
              text: "rearwards, resulting in loss of assistance.",
            },
            {
              letter: "C",
              text: "forwards, resulting in an unstable overbalance.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Aerodynamic balance (like a horn balance) positions part of the control surface forward of the hinge line. If this area is too large, the center of pressure of the control surface can move ahead of the hinge line. This creates a force that wants to further deflect the surface, leading to a dangerous condition called overbalance where the control could snap to its stop.",
        },
        {
          id: 302,
          category: "Aircraft Controls",
          text: "A flying control mass balance weight",
          options: [
            {
              letter: "A",
              text: "keeps the control surface C of G as close to the trailing edge as possible.",
            },
            {
              letter: "B",
              text: "tends to move the control surface C of G close to the hinge line.",
            },
            {
              letter: "C",
              text: "ensures that the C of G always acts to aid the pilot thus relieving control column load.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Mass balance weights are added forward of the hinge line to position the center of gravity of the control surface exactly on the hinge line. This prevents inertial forces from causing or contributing to control surface flutter.",
        },
        {
          id: 303,
          category: "Aircraft Design",
          text: "The type of flap which extends rearwards when lowered is called a",
          options: [
            {
              letter: "A",
              text: "plain flap.",
            },
            {
              letter: "B",
              text: "split flap.",
            },
            {
              letter: "C",
              text: "Fowler flap.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "A Fowler flap is distinctive because it moves aft on tracks before deflecting downward. This aft movement increases the wing area.",
        },
        {
          id: 304,
          category: "Aircraft Design",
          text: "Which of the following trailing edge flaps give an increase in wing area?",
          options: [
            {
              letter: "A",
              text: "Split flap.",
            },
            {
              letter: "B",
              text: "Fowler flap.",
            },
            {
              letter: "C",
              text: "Slotted flap.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Only the Fowler flap extends rearward, mechanically increasing the total wing area.",
        },
        {
          id: 305,
          category: "Aircraft Controls",
          text: "Which of the following is not a primary flying control?",
          options: [
            {
              letter: "A",
              text: "Elevator.",
            },
            {
              letter: "B",
              text: "Tailplane.",
            },
            {
              letter: "C",
              text: "Rudder.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "The tailplane (or horizontal stabilizer) is a fixed or variable-incidence surface that provides stability. The elevator is the primary control surface for pitch. On some aircraft with an all-moving tailplane, the entire surface becomes the primary control, but generally, the tailplane itself is not a primary control.",
        },
        {
          id: 306,
          category: "Aerodynamics",
          text: "A leading edge slat is a device for",
          options: [
            {
              letter: "A",
              text: "increasing the stalling angle of the wing.",
            },
            {
              letter: "B",
              text: "decreasing the stalling angle of the wing.",
            },
            {
              letter: "C",
              text: "decreasing wing drag.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The primary purpose of a leading-edge slat is to delay the separation of the boundary layer, allowing the wing to operate at a higher angle of attack before stalling. This increases the stalling angle.",
        },
        {
          id: 307,
          category: "Aircraft Design",
          text: "A Krueger flap is",
          options: [
            {
              letter: "A",
              text: "a flap which extends rearwards but does not lower.",
            },
            {
              letter: "B",
              text: "a leading edge flap which hinges forward.",
            },
            {
              letter: "C",
              text: "a leading edge slat which extends forward.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "A Krueger flap is a high-lift device on the leading edge. It is hinged at the front of the wing's lower surface and, when deployed, it swings down and forward from under the wing, increasing camber.",
        },
        {
          id: 308,
          category: "Aircraft Controls",
          text: "A tab which assists the pilot to move a flying control by moving automatically in the opposite direction to the control surface is called a",
          options: [
            {
              letter: "A",
              text: "servo tab.",
            },
            {
              letter: "B",
              text: "geared balance tab.",
            },
            {
              letter: "C",
              text: "trim tab.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "This is the definition of a servo tab. It is directly connected to the pilot's controls and moves opposite the main surface to generate an aerodynamic force that helps move it.",
        },
        {
          id: 309,
          category: "Aircraft Controls",
          text: "What is attached to the rear of the vertical stabilizer?",
          options: [
            {
              letter: "A",
              text: "Elevator.",
            },
            {
              letter: "B",
              text: "Aileron.",
            },
            {
              letter: "C",
              text: "Rudder.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The vertical stabilizer (fin) is the fixed vertical surface. The movable control surface hinged to its rear is the rudder.",
        },
        {
          id: 310,
          category: "Aircraft Controls",
          text: "What is fitted on the aircraft to enable the pilot to reduce his speed rapidly in event of severe turbulence, or speed tending to rise above the Never Exceed Limit?",
          options: [
            {
              letter: "A",
              text: "Lift dumpers.",
            },
            {
              letter: "B",
              text: "Air brakes.",
            },
            {
              letter: "C",
              text: "Wheel brakes.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Air brakes (or speed brakes) are high-drag devices designed to rapidly increase drag and slow the aircraft down in flight without significantly affecting the lift. Lift dumpers are for use on the ground.",
        },
        {
          id: 311,
          category: "Aircraft Controls",
          text: "When spoilers are used asymmetrically, they combine with",
          options: [
            {
              letter: "A",
              text: "ailerons.",
            },
            {
              letter: "B",
              text: "rudder.",
            },
            {
              letter: "C",
              text: "elevators.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Asymmetric spoiler deployment is a method of roll control, often used in conjunction with or in place of ailerons.",
        },
        {
          id: 312,
          category: "Aircraft Controls",
          text: "What is used to correct any tendency of the aircraft to move towards an undesirable flight attitude?",
          options: [
            {
              letter: "A",
              text: "Trim tabs.",
            },
            {
              letter: "B",
              text: "Spring tabs.",
            },
            {
              letter: "C",
              text: "Balance tabs.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Trim tabs are used to set a control surface to a position that corrects for a constant force (like a heavy wing or nose heaviness), allowing the pilot to release pressure on the controls and maintain a desired attitude.",
        },
        {
          id: 313,
          category: "Aerodynamics",
          text: "The layer of air over the surface of an aerofoil which is slower moving, in relation to the rest of the airflow, is known as",
          options: [
            {
              letter: "A",
              text: "none of the above are correct.",
            },
            {
              letter: "B",
              text: "camber layer.",
            },
            {
              letter: "C",
              text: "boundary layer.",
            },
          ],
          correctAnswer: "C",
          explanation: "This is the definition of the boundary layer.",
        },
        {
          id: 314,
          category: "Aircraft Design",
          text: "A control surface which forms a slot when deployed is called a",
          options: [
            {
              letter: "A",
              text: "slat.",
            },
            {
              letter: "B",
              text: "slot.",
            },
            {
              letter: "C",
              text: "flap.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "A slat is the movable leading-edge device. When it is deployed, it creates a slot between itself and the main wing.",
        },
        {
          id: 315,
          category: "Aircraft Controls",
          text: "Asymmetric flaps will cause",
          options: [
            {
              letter: "A",
              text: "the aircraft to descend.",
            },
            {
              letter: "B",
              text: "the aircraft to ascend.",
            },
            {
              letter: "C",
              text: "one wing to rise.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Asymmetric flaps means one flap is extended more than the other. The wing with the greater flap extension produces more lift, causing that wing to rise.",
        },
        {
          id: 316,
          category: "Aerodynamics",
          text: "When airflow velocity over an upper cambered surface of an aerofoil decreases, what takes place?",
          options: [
            {
              letter: "A",
              text: "Pressure decreases, lift increases.",
            },
            {
              letter: "B",
              text: "Pressure increases, lift decreases.",
            },
            {
              letter: "C",
              text: "Pressure increases, lift increases.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "According to Bernoulli, a decrease in velocity leads to an increase in pressure. If the pressure on the upper surface increases, the pressure difference between upper and lower surfaces decreases, resulting in a decrease in lift.",
        },
        {
          id: 317,
          category: "Aerodynamics",
          text: "What is a controlling factor of turbulence and skin friction?",
          options: [
            {
              letter: "A",
              text: "Countersunk rivets used on skin exterior.",
            },
            {
              letter: "B",
              text: "Aspect ratio.",
            },
            {
              letter: "C",
              text: "Fineness ratio.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Surface finish is a major factor in determining the amount of skin friction and where the boundary layer transitions from laminar to turbulent. Smooth surfaces like countersunk rivets reduce skin friction and delay transition.",
        },
        {
          id: 318,
          category: "Aerodynamics",
          text: "Changes in aircraft weight",
          options: [
            {
              letter: "A",
              text: "cause corresponding changes in total drag due to the associated lift change.",
            },
            {
              letter: "B",
              text: "will not affect total drag since it is dependant only upon speed.",
            },
            {
              letter: "C",
              text: "will only affect total drag if the lift is kept constant.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "A change in weight requires a change in lift to maintain level flight at a given speed. A change in lift (via angle of attack) directly affects induced drag, which is a major component of total drag. Therefore, a change in weight causes a corresponding change in total drag.",
        },
        {
          id: 319,
          category: "Aerodynamics",
          text: "When an aircraft stalls",
          options: [
            {
              letter: "A",
              text: "lift increases and drag decreases.",
            },
            {
              letter: "B",
              text: "lift and drag increase.",
            },
            {
              letter: "C",
              text: "lift decreases and drag increases.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "At the stall, the sudden separation of airflow causes a dramatic loss of lift and a large increase in drag due to the turbulent wake.",
        },
        {
          id: 320,
          category: "Aircraft Controls",
          text: "Spoiler panels are positioned so that when deployed",
          options: [
            {
              letter: "A",
              text: "roll will not occur.",
            },
            {
              letter: "B",
              text: "pitch trim is not affected.",
            },
            {
              letter: "C",
              text: "no yaw takes place.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "When used symmetrically as speed brakes, spoilers are carefully positioned and deployed to minimize any changes to the aircraft's pitch trim, allowing for a stable descent without requiring significant control input from the pilot.",
        },
        {
          id: 321,
          category: "Aerodynamics",
          text: "The aircraft stalling speed will",
          options: [
            {
              letter: "A",
              text: "only change if the MTWA were changed.",
            },
            {
              letter: "B",
              text: "be unaffected by aircraft weight changes since it is dependant upon the angle of attack.",
            },
            {
              letter: "C",
              text: "increase with an increase in weight.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Stall speed (Vs) is proportional to the square root of the wing loading (Weight/Area). Therefore, if weight increases, the stall speed increases.",
        },
        {
          id: 322,
          category: "Aerodynamics",
          text: "In a bank and turn",
          options: [
            {
              letter: "A",
              text: "extra lift is not required if thrust is increased.",
            },
            {
              letter: "B",
              text: "extra lift is not required.",
            },
            {
              letter: "C",
              text: "extra lift is required.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "In a level turn, the lift vector is tilted. The vertical component of lift must still equal weight, so the total lift must be greater than weight. This requires an increase in angle of attack, which means extra lift is required to maintain altitude.",
        },
        {
          id: 323,
          category: "Aircraft Controls",
          text: "The method employed to mass balance control surfaces is to",
          options: [
            {
              letter: "A",
              text: "fit bias strips to the trailing edge of the surfaces.",
            },
            {
              letter: "B",
              text: "attach weights forward of the hinge line.",
            },
            {
              letter: "C",
              text: "allow the leading edge of the surface to project into the airflow.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Mass balancing involves adding weights ahead of the hinge line to move the control surface's center of gravity to the hinge line. This prevents flutter.",
        },
        {
          id: 324,
          category: "Aircraft Controls",
          text: "Control surface flutter may be caused by",
          options: [
            {
              letter: "A",
              text: "excessive play in trim tab attachments.",
            },
            {
              letter: "B",
              text: "high static friction in trim tab control tabs.",
            },
            {
              letter: "C",
              text: "incorrect angular movement of trim tabs.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Flutter is an oscillation caused by the interaction of aerodynamic, inertial, and elastic forces. Excessive play or freeplay in the control system or attachments can allow oscillations to start and build up, leading to flutter.",
        },
        {
          id: 325,
          category: "Aircraft Controls",
          text: "A differential aileron control system results in",
          options: [
            {
              letter: "A",
              text: "aileron drag being reduced on the inner wing in a turn.",
            },
            {
              letter: "B",
              text: "aileron drag being reduced on the outer wing in a turn.",
            },
            {
              letter: "C",
              text: "aileron drag being compensated by small rudder movements.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "In a turn, the outer wing travels faster and generates more lift. Differential ailerons, by having the up-going aileron deflect more, increase drag on the inner (down-going) wing, which helps to pull the nose into the turn. This reduces the drag on the outer wing, thus reducing adverse yaw.",
        },
        {
          id: 326,
          category: "Aerodynamics",
          text: "The primary function of a flap is",
          options: [
            {
              letter: "A",
              text: "to trim the aircraft longitudinally.",
            },
            {
              letter: "B",
              text: "to alter the position of the centre of gravity.",
            },
            {
              letter: "C",
              text: "to alter the lift of an aerofoil.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "The primary function of flaps is to increase the lift coefficient (CLmax) of the wing, allowing for slower flight during takeoff and landing.",
        },
        {
          id: 327,
          category: "Aerodynamics",
          text: "The angle of attack at which stall occurs",
          options: [
            {
              letter: "A",
              text: "can be varied by using flaps and slats.",
            },
            {
              letter: "B",
              text: "depends on the weight of the aircraft.",
            },
            {
              letter: "C",
              text: "cannot be varied, it is always constant.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The critical angle of attack at which stall occurs is not fixed; it can be changed by high-lift devices. Flaps and slats allow the wing to reach a higher angle of attack before stalling, effectively increasing the stalling angle.",
        },
        {
          id: 328,
          category: "Aerodynamics",
          text: "The stalling speed of an aircraft",
          options: [
            {
              letter: "A",
              text: "is increased when it is heavier.",
            },
            {
              letter: "B",
              text: "does not change.",
            },
            {
              letter: "C",
              text: "is increased when it is lighter.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Stall speed increases with weight (Vs ∝ √Weight). A heavier aircraft needs to fly faster to generate the same lift at the same angle of attack.",
        },
        {
          id: 329,
          category: "Aircraft Controls",
          text: "A wing flap which has dropped or partially extended on one wing in flight will lead to",
          options: [
            {
              letter: "A",
              text: "a fixed banked attitude which would be corrected by use of the rudder.",
            },
            {
              letter: "B",
              text: "a pitching moment which would be corrected by used of the elevators.",
            },
            {
              letter: "C",
              text: "a steady rolling tendency which would be corrected by use of the ailerons.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Asymmetric flaps will create a significant difference in lift between the two wings. This results in a steady rolling tendency towards the wing with the less-extended (or retracted) flap. The pilot would need to use ailerons to counteract this roll.",
        },
        {
          id: 330,
          category: "Aerodynamics",
          text: "With an increase in the amount of flap deployment, the stalling angle of a wing",
          options: [
            { letter: "A", text: "remains the same." },
            { letter: "B", text: "increases." },
            { letter: "C", text: "decreases." },
          ],
          correctAnswer: "C",
          explanation:
            "While flaps increase the maximum lift coefficient (C_Lmax), they typically decrease the stalling angle (the angle of attack at which stall occurs). This is because they increase the camber, which makes the airflow more prone to separation at a lower angle.",
        },
        {
          id: 331,
          category: "Aircraft Controls",
          text: "Aerodynamic balance of a control surface may be achieved",
          options: [
            {
              letter: "A",
              text: "by a horn at the extremity of the surface forward of the hinge line.",
            },
            {
              letter: "B",
              text: "by weights added to the control surface aft of the hinge line.",
            },
            {
              letter: "C",
              text: "by a trimming strip at the trailing edge of the surface.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Aerodynamic balance reduces the force needed to move a control surface. A horn balance is a projection of the surface ahead of the hinge line. The airflow on this horn creates a force that helps move the surface.",
        },
        {
          id: 332,
          category: "Aircraft Controls",
          text: "A control surface is provided with aerodynamic balancing to",
          options: [
            { letter: "A", text: "assist the pilot in moving the control." },
            { letter: "B", text: "increase stability." },
            {
              letter: "C",
              text: "decrease the drag when the control is deflected.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "The primary purpose of aerodynamic balancing (e.g., horn balances, balance tabs) is to reduce the hinge moment, making it easier for the pilot to move the control surface, especially at higher airspeeds.",
        },
        {
          id: 333,
          category: "Aerodynamics",
          text: "Downward displacement of an aileron",
          options: [
            {
              letter: "A",
              text: "increases the angle at which its wing stalls.",
            },
            {
              letter: "B",
              text: "decreases the angle at which its wing will stall.",
            },
            {
              letter: "C",
              text: "has no effect on its wing stalling angle, it only affects the stalling speed on that wing.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "A downward-deflected aileron increases the camber and effective angle of attack of that portion of the wing. This brings that section closer to its critical angle of attack, effectively decreasing its stall margin and making it stall at a lower geometric angle of attack.",
        },
        {
          id: 334,
          category: "Aerodynamics",
          text: "Due to the tailplane angle of attack change, the flap-induced downwash on the tailplane",
          options: [
            {
              letter: "A",
              text: "will tend to cause an aircraft nose-up pitch.",
            },
            {
              letter: "B",
              text: "may cause a nose-down or nose-up pitch depending upon the initial tailplane load.",
            },
            {
              letter: "C",
              text: "will tend to cause an aircraft nose down pitch.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "Flap deployment increases downwash. This changes the angle of attack at the tailplane. If the tailplane normally produces a downforce, the increased downwash will increase that downforce, causing a nose-up pitch. If the tailplane produces an upload, the effect may be opposite. The result depends on the tailplane's design and the aircraft's CG position.",
        },
        {
          id: 335,
          category: "Aerodynamics",
          text: "Due to the change in lift coefficient accompanying extension of the flaps, to maintain the lift constant it would be necessary to",
          options: [
            { letter: "A", text: "raise the nose." },
            { letter: "B", text: "lower the nose." },
            { letter: "C", text: "keep the pitch attitude constant." },
          ],
          correctAnswer: "B",
          explanation:
            "Extending flaps increases the lift coefficient (C_L). To maintain the same total lift (which equals weight in level flight), the aircraft must reduce its angle of attack to lower the C_L back to the required value. This is done by lowering the nose.",
        },
        {
          id: 336,
          category: "Aircraft Controls",
          text: "A differential aileron control is one which gives",
          options: [
            {
              letter: "A",
              text: "the down-going aileron more travel than the up-going one.",
            },
            {
              letter: "B",
              text: "equal aileron travel in each direction, but variable for stick movement.",
            },
            { letter: "C", text: "a larger aileron up travel than down." },
          ],
          correctAnswer: "C",
          explanation:
            "Differential ailerons are designed so that the up-going aileron deflects more than the down-going one. This helps to reduce adverse yaw by creating more drag on the downward-going wing.",
        },
        {
          id: 337,
          category: "Aerodynamics",
          text: "Which leading edge device improves the laminar flow over the wing?",
          options: [
            { letter: "A", text: "Flap and slat." },
            { letter: "B", text: "Slat." },
            { letter: "C", text: "Flap." },
          ],
          correctAnswer: "B",
          explanation:
            "A slat, by creating a slot, directs high-energy air onto the upper surface. This re-energizes the boundary layer, helping it remain attached (laminar or turbulent attached) over a greater portion of the wing, thus improving lift and delaying stall. It doesn't necessarily keep it laminar, but improves its health.",
        },
        {
          id: 338,
          category: "Aircraft Controls",
          text: "The balance tab is an auxiliary surface fitted to a main control surface",
          options: [
            {
              letter: "A",
              text: "operating automatically to assist the pilot in moving the controls.",
            },
            {
              letter: "B",
              text: "operated independently at which point in the length of cable the tensiometer is applied.",
            },
            {
              letter: "C",
              text: "operating automatically to provide feel to the controls.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "A balance tab is linked to the fixed surface or the structure ahead of the control. When the pilot moves the main control, the tab is forced to move in the opposite direction. The aerodynamic force on the tab then helps to move the main surface, reducing pilot effort. It operates automatically with control movement.",
        },
        {
          id: 339,
          category: "Aircraft Controls",
          text: "Aerodynamic balancing of flight controls is achieved by",
          options: [
            { letter: "A", text: "placing a weight ahead of the hinge point." },
            {
              letter: "B",
              text: "placing a weight in the leading edge of the control surface.",
            },
            {
              letter: "C",
              text: "providing a portion of the control surface ahead of the hinge point.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Aerodynamic balance uses air loads to assist the pilot. This is achieved by extending a portion of the control surface (like a horn) forward of the hinge line, so that airflow on this portion creates a moment that aids deflection.",
        },
        {
          id: 340,
          category: "Aircraft Controls",
          text: "Aerodynamic balance is used to",
          options: [
            { letter: "A", text: "reduce the control load to zero." },
            { letter: "B", text: "make the flying controls easier to move." },
            { letter: "C", text: "prevent flutter of the flying controls." },
          ],
          correctAnswer: "B",
          explanation:
            "The primary purpose of aerodynamic balance is to reduce the hinge moment, thereby reducing the physical force the pilot must exert to move the control surface.",
        },
        {
          id: 341,
          category: "Aircraft Controls",
          text: "A horn balance is",
          options: [
            {
              letter: "A",
              text: "a rod projecting forward from the control surface with a weight on the end.",
            },
            {
              letter: "B",
              text: "a rod projecting upward from the main control surface to which the control cables are attached.",
            },
            {
              letter: "C",
              text: "a projection of the outer edge of the control surface forward of the hinge line.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "A horn balance is a physical extension of the control surface's tip or edge that protrudes forward of the hinge line. This area is exposed to airflow, and when the surface is deflected, the air pressure on the 'horn' creates a force that assists the movement.",
        },
        {
          id: 342,
          category: "Aircraft Controls",
          text: "A control surface is mass balanced by",
          options: [
            {
              letter: "A",
              text: "the attachment of weights acting on the hinge line.",
            },
            { letter: "B", text: "fitting a balance tab." },
            {
              letter: "C",
              text: "the attachment of weights acting forward of the hinge line.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Mass balancing involves adding weights forward of the hinge line. This positions the center of gravity of the control surface at the hinge line, preventing inertial forces from inducing or contributing to flutter.",
        },
        {
          id: 343,
          category: "Aircraft Controls",
          text: "The purpose of anti-balance tabs is to",
          options: [
            { letter: "A", text: "relieve stick loads." },
            { letter: "B", text: "trim the aircraft." },
            { letter: "C", text: "give more feel to the control column." },
          ],
          correctAnswer: "C",
          explanation:
            "An anti-balance (or anti-servo) tab moves in the same direction as the main control surface. This increases the force needed to move the control, providing the pilot with artificial feel or feedback, especially on aircraft with fully powered controls where natural aerodynamic forces are absent.",
        },
        {
          id: 344,
          category: "Aircraft Controls",
          text: "You have adjusted the elevator trim tab to correct for nose heavy. What was the direction of travel of the trim tab?",
          options: [
            { letter: "A", text: "The elevator trim tab has moved down." },
            { letter: "B", text: "The elevator trim tab has moved up." },
            {
              letter: "C",
              text: "The port elevator tab has moved up and starboard moved down.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "Nose heavy means the pilot must hold back pressure (elevator up) to keep the nose up. The trim tab moves opposite to the desired control surface position. To hold the elevator in the up position, the trim tab must be deflected down.",
        },
        {
          id: 345,
          category: "ISA/Atmosphere",
          text: "The tropopause exists at about",
          options: [
            { letter: "A", text: "18,000 ft." },
            { letter: "B", text: "30,000 ft." },
            { letter: "C", text: "36,000 ft." },
          ],
          correctAnswer: "C",
          explanation:
            "In the International Standard Atmosphere (ISA), the tropopause, the boundary between the troposphere and stratosphere, is located at approximately 36,000 feet (11 km).",
        },
        {
          id: 346,
          category: "Aerodynamics",
          text: "Induced drag curve characteristics of a slender delta wing are such that there is",
          options: [
            { letter: "A", text: "an increase in gradient with wing speed." },
            { letter: "B", text: "no change in gradient with wing speed." },
            { letter: "C", text: "decrease in gradient with wing speed." },
          ],
          correctAnswer: "A",
          explanation:
            "Slender delta wings generate lift through vortices. At higher angles of attack (lower speeds), these vortices are strong and produce a non-linear lift increase. This affects the induced drag, causing its curve to have a different, often steeper, gradient compared to conventional wings.",
        },
        {
          id: 347,
          category: "Aircraft Controls",
          text: "If an aircraft is yawing left, the trim tab on the rudder would be positioned",
          options: [
            { letter: "A", text: "to the right, moving the rudder left." },
            { letter: "B", text: "to the centre." },
            { letter: "C", text: "to the left, moving the rudder right." },
          ],
          correctAnswer: "A",
          explanation:
            "To correct a left yaw, the rudder must be deflected to the right. Trim tabs move opposite to the desired rudder position. Therefore, to hold the rudder to the right, the trim tab must be positioned to the left.",
        },
        {
          id: 348,
          category: "Stability",
          text: "Instability giving roll and yaw",
          options: [
            { letter: "A", text: "is dutch roll." },
            { letter: "B", text: "is longitudinal stability." },
            { letter: "C", text: "is lateral stability." },
          ],
          correctAnswer: "A",
          explanation:
            "Dutch roll is a dynamic instability involving coupled oscillations in both roll and yaw.",
        },
        {
          id: 349,
          category: "Aerodynamics",
          text: "Vortex generators are fitted to",
          options: [
            { letter: "A", text: "move transition point rearwards." },
            { letter: "B", text: "move transition point forwards." },
            { letter: "C", text: "advance the onset of flow separation." },
          ],
          correctAnswer: "A",
          explanation:
            "Vortex generators are small vanes that create vortices. These vortices mix high-energy air from outside the boundary layer into the slow-moving boundary layer, re-energizing it. This helps to delay flow separation and can effectively move the transition point (to turbulent flow) rearward by keeping the flow attached longer.",
        },
        {
          id: 350,
          category: "Aerodynamics",
          text: "Leading edge flaps",
          options: [
            { letter: "A", text: "increase stalling angle of the wing." },
            { letter: "B", text: "decrease stalling angle of the wing." },
            { letter: "C", text: "do not change the stalling angle." },
          ],
          correctAnswer: "A",
          explanation:
            "Leading edge devices like flaps or slats increase the camber of the wing's leading edge and/or create a slot. This allows the airflow to remain attached to the upper surface at higher angles of attack, thereby increasing the stalling angle.",
        },
        {
          id: 351,
          category: "Aircraft Design",
          text: "Krueger flaps are on",
          options: [
            { letter: "A", text: "the leading edge." },
            { letter: "B", text: "either the leading or training edge." },
            { letter: "C", text: "the trailing edge." },
          ],
          correctAnswer: "A",
          explanation:
            "Krueger flaps are a type of high-lift device mounted on the leading edge of the wing. They hinge forward from the lower surface.",
        },
        {
          id: 352,
          category: "Stability",
          text: "Sweepback will",
          options: [
            { letter: "A", text: "decrease lateral stability." },
            { letter: "B", text: "not affect lateral stability." },
            { letter: "C", text: "increase lateral stability." },
          ],
          correctAnswer: "C",
          explanation:
            "Sweepback contributes to lateral (roll) stability. In a sideslip, the forward-swept wing presents a longer span perpendicular to the airflow, generating more lift, which tends to roll the aircraft back to level.",
        },
        {
          id: 353,
          category: "Aircraft Design",
          text: "A plain flap",
          options: [
            {
              letter: "A",
              text: "does not increase the wing area on deployment.",
            },
            {
              letter: "B",
              text: "is attached to the leading edge of the wing.",
            },
            { letter: "C", text: "forms part of lower trailing edge." },
          ],
          correctAnswer: "A",
          explanation:
            "A plain flap is simply the hinged rear portion of the wing. It increases camber but does not extend rearward, so it does not increase the wing area.",
        },
        {
          id: 354,
          category: "Aerodynamics",
          text: "A split flap, when deployed",
          options: [
            { letter: "A", text: "is used only on high speed aircraft." },
            {
              letter: "B",
              text: "increases lift without a corresponding increase in drag.",
            },
            {
              letter: "C",
              text: "increases drag with little lift coefficient increase, from intermediate to fully down.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "A split flap deflects from the lower surface only. It creates a turbulent area behind the wing, which significantly increases drag. While it does increase lift, the drag increase is proportionally larger, especially at higher deflection angles.",
        },
        {
          id: 355,
          category: "Aircraft Controls",
          text: "A flying control mass balance weight",
          options: [
            {
              letter: "A",
              text: "keeps the control surface C of G as close to the trailing edge as possible.",
            },
            {
              letter: "B",
              text: "tends to move the control surface C of G close to the hinge line.",
            },
            {
              letter: "C",
              text: "tends to move the control surface C of G forward of the hinge line.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "Mass balance weights are added forward of the hinge line. Their purpose is to position the center of gravity of the control surface at the hinge line. To achieve this, they must move the overall CG forward, often to a point exactly on the hinge line.",
        },
        {
          id: 356,
          category: "Aircraft Controls",
          text: "An elevator controls the aircraft motion in",
          options: [
            { letter: "A", text: "yaw." },
            { letter: "B", text: "roll." },
            { letter: "C", text: "pitch." },
          ],
          correctAnswer: "C",
          explanation:
            "The elevator controls rotation about the lateral axis, which is pitching.",
        },
        {
          id: 357,
          category: "Aerodynamics",
          text: "Air above Mach 0.7 is",
          options: [
            {
              letter: "A",
              text: "compressible only when above the speed of sound.",
            },
            { letter: "B", text: "incompressible." },
            { letter: "C", text: "compressible." },
          ],
          correctAnswer: "C",
          explanation:
            "Above approximately Mach 0.3, compressibility effects become noticeable. By Mach 0.7, these effects are significant, and the air can no longer be treated as incompressible for accurate aerodynamic calculations.",
        },
        {
          id: 358,
          category: "Aerodynamics",
          text: "Supersonic air passing through a divergent duct causes the",
          options: [
            {
              letter: "A",
              text: "pressure to increase, velocity to increase.",
            },
            {
              letter: "B",
              text: "pressure to increase, velocity to decrease.",
            },
            {
              letter: "C",
              text: "pressure to decrease, velocity to increase.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "For supersonic flow (M > 1), the rules are reversed from subsonic flow. In a divergent duct (increasing area), supersonic flow expands, causing velocity to increase and pressure and density to decrease.",
        },
        {
          id: 359,
          category: "Aerodynamics",
          text: "A nose down change of trim (tuck-under) occurs due to shock induced",
          options: [
            { letter: "A", text: "tip stall on a delta wing aircraft." },
            { letter: "B", text: "root stall on a delta wing aircraft." },
            { letter: "C", text: "tip stall on a straight wing aircraft." },
          ],
          correctAnswer: "A",
          explanation:
            "On a swept wing, tuck-under can occur when shock waves cause the inboard section of the wing to lose lift (effectively stalling) and the outboard section to generate more lift, shifting the center of pressure forward. For a delta wing, tip stall moves CP forward, causing a nose-down moment.",
        },
        {
          id: 360,
          category: "Aerodynamics",
          text: "A symmetrical aerofoil is accelerating through Mach 1 with an angle of attack of 0°. A shock wave will form",
          options: [
            {
              letter: "A",
              text: "on the upper and lower surface and will move aft until the point of maximum camber.",
            },
            {
              letter: "B",
              text: "on the upper and lower surface and will move aft.",
            },
            { letter: "C", text: "on the upper surface only and move aft." },
          ],
          correctAnswer: "B",
          explanation:
            "At 0° angle of attack, a symmetrical airfoil presents the same shape to the airflow above and below. As it accelerates through Mach 1, shock waves will form symmetrically on both the upper and lower surfaces. As speed increases further, these shocks will move aft towards the trailing edge.",
        },
        {
          id: 361,
          category: "Aerodynamics",
          text: "Shock stall",
          options: [
            { letter: "A", text: "occurs at high speeds." },
            {
              letter: "B",
              text: "is a flap down stall and occurs at high speeds.",
            },
            { letter: "C", text: "occurs at low speeds." },
          ],
          correctAnswer: "A",
          explanation:
            "Shock stall is a high-speed phenomenon that occurs in the transonic region. It is caused by the formation of shock waves on the wing, which can cause flow separation and a loss of lift, analogous to a low-speed stall but at high Mach numbers.",
        },
        {
          id: 362,
          category: "Aerodynamics",
          text: "As you approach supersonic speed",
          options: [
            { letter: "A", text: "thrust is reduced." },
            { letter: "B", text: "total drag is increased." },
            { letter: "C", text: "lift is reduced." },
          ],
          correctAnswer: "B",
          explanation:
            "As an aircraft approaches the speed of sound (transonic region), it encounters a rapid and significant increase in total drag, primarily due to the onset of wave drag. This is known as the sound barrier.",
        },
        {
          id: 363,
          category: "Aircraft Controls",
          text: "Mach trim in some aircraft assists",
          options: [
            { letter: "A", text: "lateral stability." },
            { letter: "B", text: "vertical stability." },
            { letter: "C", text: "longitudinal stability." },
          ],
          correctAnswer: "C",
          explanation:
            "At high Mach numbers, the center of pressure can shift, causing a pitch change (like tuck-under). Mach trim systems automatically adjust the stabilizer or elevator to counteract this pitch tendency, maintaining longitudinal stability and trim.",
        },
        {
          id: 364,
          category: "Aerodynamics",
          text: "Before an aircraft reaches critical mach",
          options: [
            {
              letter: "A",
              text: "the nose pitches up because the CP moves Forward.",
            },
            {
              letter: "B",
              text: "the aircraft buffets because the CP moves to the shock wave.",
            },
            {
              letter: "C",
              text: "the nose pitches down because the CP moves rear.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "As the aircraft approaches the critical Mach number, local supersonic flow and shock waves start to form. This can initially cause the center of pressure to move forward, leading to a nose-up pitching moment.",
        },
        {
          id: 365,
          category: "Aerodynamics",
          text: "On a standard day, at which altitude will the speed of sound be the greatest?",
          options: [
            { letter: "A", text: "20,000 ft." },
            { letter: "B", text: "10,000 ft." },
            { letter: "C", text: "Sea level." },
          ],
          correctAnswer: "C",
          explanation:
            "The speed of sound is directly proportional to the square root of the absolute temperature. On a standard day, temperature is highest at sea level (15°C) and decreases with altitude in the troposphere. Therefore, the speed of sound is greatest at sea level.",
        },
        {
          id: 366,
          category: "Aerodynamics",
          text: "Which of the following will increase the Critical Mach Number of an aerofoil?",
          options: [
            {
              letter: "A",
              text: "Using a thin airfoil and sweeping the wings back.",
            },
            {
              letter: "B",
              text: "Decreasing the fineness ratio of the wings.",
            },
            { letter: "C", text: "Increasing the aspect ratio of the wings." },
          ],
          correctAnswer: "A",
          explanation:
            "The Critical Mach Number (Mcrit) is increased by delaying the onset of local supersonic flow. This is achieved by using thinner airfoils (reducing the peak suction) and sweeping the wings back (reducing the component of flow perpendicular to the leading edge).",
        },
        {
          id: 367,
          category: "Aerodynamics",
          text: "As an aircraft accelerates through the transonic region, the centre of pressure tends to",
          options: [
            { letter: "A", text: "turn into a shock wave." },
            { letter: "B", text: "move rearward." },
            { letter: "C", text: "move forward." },
          ],
          correctAnswer: "B",
          explanation:
            "In the transonic region, the formation and movement of shock waves cause the center of pressure to shift. Typically, as speed increases through the transonic region, the CP moves rearward. This can cause a nose-down pitching moment known as 'tuck-under'.",
        },
        {
          id: 368,
          category: "Aerodynamics",
          text: "Supersonic air going through an incipient shock wave will decrease its speed and",
          options: [
            { letter: "A", text: "decrease temperature and increase density." },
            { letter: "B", text: "increase temperature and decrease density." },
            { letter: "C", text: "increase temperature and increase density." },
          ],
          correctAnswer: "C",
          explanation:
            "A shock wave is a very thin region across which the flow properties change dramatically and irreversibly. As supersonic flow passes through a shock, it slows down to subsonic speed, and its pressure, temperature, and density all increase.",
        },
        {
          id: 369,
          category: "Aerodynamics",
          text: "An increase in mach number will cause the",
          options: [
            {
              letter: "A",
              text: "CofP to move rearwards giving more downwash on the tail plane.",
            },
            {
              letter: "B",
              text: "CofP to move forwards giving less downwash on the tail plane.",
            },
            {
              letter: "C",
              text: "CofP to move rearwards giving less downwash on the tail plane.",
            },
          ],
          correctAnswer: "C",
          explanation:
            "As Mach number increases into the transonic region, the CP typically moves aft. This aft movement can change the downwash angle at the tailplane, often reducing it.",
        },
        {
          id: 370,
          category: "Aerodynamics",
          text: "At speeds above Mach 1, shockwaves will form above and below the wing",
          options: [
            { letter: "A", text: "at the trailing edge." },
            {
              letter: "B",
              text: "at both the leading edge and the trailing edge.",
            },
            { letter: "C", text: "at the leading edge." },
          ],
          correctAnswer: "C",
          explanation:
            "At supersonic speeds, the air cannot 'warn' the air ahead of the wing's presence. This causes shock waves to form at the leading edge (bow shock) and often at other discontinuities, but the initial and defining shock for supersonic flight is at the leading edge.",
        },
        {
          id: 371,
          category: "Aerodynamics",
          text: "Above the critical mach number, the drag coefficient",
          options: [
            { letter: "A", text: "increases." },
            { letter: "B", text: "remains the same." },
            { letter: "C", text: "decreases." },
          ],
          correctAnswer: "A",
          explanation:
            "The critical Mach number is the point where airflow somewhere on the aircraft first reaches sonic speed. Above this speed, shock waves form and strengthen, causing a dramatic increase in the drag coefficient, known as wave drag.",
        },
        {
          id: 372,
          category: "Aircraft Controls",
          text: "Mach trim counters",
          options: [
            { letter: "A", text: "longitudinal instability." },
            { letter: "B", text: "vertical instability." },
            { letter: "C", text: "lateral instability." },
          ],
          correctAnswer: "A",
          explanation:
            "Mach trim systems are designed to counteract pitch trim changes (tuck-under or nose-up trim) that occur at high Mach numbers, thereby maintaining longitudinal stability and reducing pilot workload.",
        },
        {
          id: 373,
          category: "Aerodynamics",
          text: "At high Mach Numbers above Mach 2.2, some aircraft metals",
          options: [
            { letter: "A", text: "such as aluminium, become brittle." },
            {
              letter: "B",
              text: "lose their strength due to the kinetic heating effect.",
            },
            {
              letter: "C",
              text: "will shrink due to the extreme pressures involved.",
            },
          ],
          correctAnswer: "B",
          explanation:
            "At very high supersonic speeds, air friction causes extreme temperatures on the aircraft's skin (kinetic heating). This can severely weaken traditional aircraft metals like aluminum, necessitating the use of heat-resistant materials like titanium or stainless steel.",
        },
        {
          id: 374,
          category: "Aircraft Controls",
          text: "Mach trim operates",
          options: [
            { letter: "A", text: "along the longitudinal axis." },
            { letter: "B", text: "along the lateral axis." },
            { letter: "C", text: "to reduce Dutch roll." },
          ],
          correctAnswer: "A",
          explanation:
            "Mach trim affects the aircraft's pitch attitude, which is movement about the lateral axis. However, it is often described as operating to maintain stability along the longitudinal plane. The more direct answer is that it maintains longitudinal stability.",
        },
        {
          id: 375,
          category: "Aerodynamics",
          text: "To increase critical mach number",
          options: [
            { letter: "A", text: "the wings are swept." },
            { letter: "B", text: "elevons are fitted." },
            { letter: "C", text: "tailerons are fitted." },
          ],
          correctAnswer: "A",
          explanation:
            "Sweeping the wings back is a primary method of increasing the critical Mach number. It reduces the component of airflow perpendicular to the leading edge, effectively making the wing behave as if it were thinner at a given Mach number.",
        },
        {
          id: 376,
          category: "Aerodynamics",
          text: "When approaching the speed of sound the",
          options: [
            {
              letter: "A",
              text: "pressure above the wing exceeds the pressure below the wing in places.",
            },
            {
              letter: "B",
              text: "pressure above the wing can never exceed the pressure below the wing.",
            },
            {
              letter: "C",
              text: "pressure above the wing equals the pressure below the wing.",
            },
          ],
          correctAnswer: "A",
          explanation:
            "As the aircraft approaches the speed of sound, local airflow over the curved upper surface can become supersonic, forming shock waves. Behind these shocks, the pressure can recover to values that are locally higher than the pressure on the lower surface at that same chordwise position.",
        },
        {
          id: 377,
          category: "Aerodynamics",
          text: "Airspeeds above the speed of sound, but not exceeding 4 times the speed of sound are",
          options: [
            { letter: "A", text: "supersonic." },
            { letter: "B", text: "hypersonic." },
            { letter: "C", text: "hyposonic." },
          ],
          correctAnswer: "A",
          explanation:
            "Supersonic speeds are generally defined as speeds greater than Mach 1. Hypersonic speeds typically start around Mach 5.",
        },
        {
          id: 378,
          category: "Aerodynamics",
          text: "An aircraft experiences a large loss of lift and a big increase in drag in straight and level flight, what would be the most probable cause?",
          options: [
            { letter: "A", text: "Atmospheric conditions." },
            { letter: "B", text: "Aircraft reached its critical mach number." },
            { letter: "C", text: "Severe head winds." },
          ],
          correctAnswer: "B",
          explanation:
            "When an aircraft reaches its critical Mach number and enters the transonic drag rise, it can experience a severe increase in drag and",
        },
      ];

      // Estado del examen
      let currentQuestionIndex = 0;
      let userAnswers = new Array(questions.length).fill(null);
      let checkedAnswers = new Array(questions.length).fill(false);
      let questionStatus = new Array(questions.length).fill("unanswered"); // 'unanswered', 'answered', 'correct', 'incorrect'
      let correctAnswers = 0;
      let incorrectAnswers = 0;

      // Inicializar
      function init() {
        createQuestionIndex();
        loadQuestion(currentQuestionIndex);
        updateStats();
      }

      // Crear índice de preguntas
      function createQuestionIndex() {
        const indexContainer = document.getElementById("questionIndex");
        indexContainer.innerHTML = "";

        questions.forEach((question, index) => {
          const item = document.createElement("div");
          item.className = "index-item";
          item.dataset.index = index;

          const statusDot = document.createElement("div");
          statusDot.className = "status-dot";

          item.innerHTML = `
                          <span>Pregunta ${index + 1}</span>
                      `;
          item.appendChild(statusDot);

          item.onclick = function () {
            currentQuestionIndex = parseInt(this.dataset.index);
            loadQuestion(currentQuestionIndex);
            updateQuestionIndex();
          };

          indexContainer.appendChild(item);
        });

        updateQuestionIndex();
      }

      // Actualizar índice de preguntas
      function updateQuestionIndex() {
        const items = document.querySelectorAll(".index-item");
        const statusDots = document.querySelectorAll(".status-dot");

        items.forEach((item, index) => {
          item.classList.remove("active");
          if (index === currentQuestionIndex) {
            item.classList.add("active");
          }

          // Actualizar estado del punto
          statusDots[index].className = "status-dot";
          if (questionStatus[index] === "correct") {
            statusDots[index].classList.add("correct");
          } else if (questionStatus[index] === "incorrect") {
            statusDots[index].classList.add("incorrect");
          } else if (userAnswers[index] !== null) {
            statusDots[index].classList.add("answered");
          }
        });
      }

      // Cargar pregunta
      function loadQuestion(index) {
        const question = questions[index];

        // Actualizar número de pregunta
        document.getElementById("currentQuestion").textContent = index + 1;
        document.getElementById("totalQuestions").textContent =
          questions.length;

        // Mostrar texto de la pregunta
        document.getElementById("questionText").textContent = question.text;

        // Crear opciones
        const optionsContainer = document.getElementById("optionsContainer");
        optionsContainer.innerHTML = "";

        question.options.forEach((option) => {
          const optionDiv = document.createElement("div");
          optionDiv.className = "option";
          optionDiv.dataset.letter = option.letter;

          // Marcar si está seleccionada
          if (userAnswers[index] === option.letter) {
            optionDiv.classList.add("selected");
          }

          // Si ya se verificó, mostrar colores
          if (checkedAnswers[index]) {
            if (option.letter === question.correctAnswer) {
              optionDiv.classList.add("correct");
            } else if (
              userAnswers[index] === option.letter &&
              userAnswers[index] !== question.correctAnswer
            ) {
              optionDiv.classList.add("incorrect");
            }
          }

          optionDiv.innerHTML = `
                          <div class="option-letter">${option.letter}</div>
                          <div class="option-text">${option.text}</div>
                      `;

          // Solo permitir seleccionar si no se ha verificado
          if (!checkedAnswers[index]) {
            optionDiv.onclick = function () {
              selectOption(this.dataset.letter);
            };
          }

          optionsContainer.appendChild(optionDiv);
        });

        // Mostrar retroalimentación si ya se verificó
        const feedback = document.getElementById("feedback");
        if (checkedAnswers[index]) {
          const isCorrect = userAnswers[index] === question.correctAnswer;
          feedback.className = `feedback ${
            isCorrect ? "correct" : "incorrect"
          } show`;
          feedback.innerHTML = isCorrect
            ? `<strong>¡Correcto!</strong> ${question.explanation}`
            : `<strong>Incorrecto.</strong> Tu respuesta: ${userAnswers[index]}. La respuesta correcta es ${question.correctAnswer}. ${question.explanation}`;
        } else {
          feedback.className = "feedback";
          feedback.innerHTML = "";
        }

        // Actualizar botones
        updateButtons();
        updateQuestionIndex();
      }

      // Seleccionar opción
      function selectOption(optionLetter) {
        // Quitar selección anterior
        const options = document.querySelectorAll(".option");
        options.forEach((opt) => {
          opt.classList.remove("selected");
        });

        // Marcar nueva selección
        const selectedOption = document.querySelector(
          `.option[data-letter="${optionLetter}"]`
        );
        if (selectedOption) {
          selectedOption.classList.add("selected");
        }

        // Guardar respuesta
        userAnswers[currentQuestionIndex] = optionLetter;

        // Actualizar estado
        questionStatus[currentQuestionIndex] = "answered";

        // Actualizar estadísticas
        updateStats();
        updateQuestionIndex();
      }

      // Verificar respuesta
      function checkAnswer() {
        if (userAnswers[currentQuestionIndex] === null) {
          alert("Por favor, selecciona una respuesta primero.");
          return;
        }

        const question = questions[currentQuestionIndex];
        const isCorrect =
          userAnswers[currentQuestionIndex] === question.correctAnswer;

        // Marcar como verificada
        checkedAnswers[currentQuestionIndex] = true;

        // Actualizar estado
        questionStatus[currentQuestionIndex] = isCorrect
          ? "correct"
          : "incorrect";

        // Actualizar contadores
        if (isCorrect) {
          correctAnswers++;
        } else {
          incorrectAnswers++;
        }

        // Mostrar retroalimentación
        const feedback = document.getElementById("feedback");
        feedback.className = `feedback ${
          isCorrect ? "correct" : "incorrect"
        } show`;
        feedback.innerHTML = isCorrect
          ? `<strong>¡Correcto!</strong> ${question.explanation}`
          : `<strong>Incorrecto.</strong> Tu respuesta: ${userAnswers[currentQuestionIndex]}. La respuesta correcta es ${question.correctAnswer}. ${question.explanation}`;

        // Recargar pregunta para mostrar colores
        loadQuestion(currentQuestionIndex);

        // Actualizar estadísticas
        updateStats();
      }

      // Actualizar botones
      function updateButtons() {
        const prevBtn = document.getElementById("prevBtn");
        const nextBtn = document.getElementById("nextBtn");
        const checkBtn = document.getElementById("checkBtn");

        prevBtn.disabled = currentQuestionIndex === 0;
        nextBtn.disabled = currentQuestionIndex === questions.length - 1;
        checkBtn.disabled = checkedAnswers[currentQuestionIndex];
      }

      // Pregunta anterior
      function prevQuestion() {
        if (currentQuestionIndex > 0) {
          currentQuestionIndex--;
          loadQuestion(currentQuestionIndex);
        }
      }

      // Siguiente pregunta
      function nextQuestion() {
        if (currentQuestionIndex < questions.length - 1) {
          currentQuestionIndex++;
          loadQuestion(currentQuestionIndex);
        }
      }

      // Actualizar estadísticas
      function updateStats() {
        const answered = userAnswers.filter((answer) => answer !== null).length;
        const unanswered = questions.length - answered;

        document.getElementById("correctCount").textContent = correctAnswers;
        document.getElementById("incorrectCount").textContent =
          incorrectAnswers;
        document.getElementById("unansweredCount").textContent = unanswered;
      }

      // Iniciar cuando se cargue la página
      window.onload = init;
    </script>
  </body>
</html>
