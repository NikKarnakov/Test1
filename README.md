┌──────────────────────┐          ┌────────────────────────────────────┐  
│    Пользователь      │  HTTP    │            Frontend (JS, HTML)     │  
│ (браузер, формы)     ├─────────▶│ • index.html + Bootstrap, Plotly   │  
└──────────────────────┘   POST   │ • jsPDF для формирования PDF       │  
                               │ • fetch /api/reley_hugoniot и др.   │  
                               └────────────────────────────────────┘  
                                            │  
                                            │ JSON (POST)  
                                            ▼  
                               ┌────────────────────────────────────┐  
                               │       Web-сервер (Flask/FastAPI)    │  
                               │ • Принимает данные из fetch        │  
                               │ • Вызов соответствующей функции     │  
                               │   (solve_*) из backend-модуля       │  
                               └────────────────────────────────────┘  
                                            │  
                                            │ Вызов функций Python  
                                            ▼  
┌────────────────────────────────────────────────────────────────────────────┐  
│                             backend (Python)                               │  
│  ┌──────────────────────────────────────────────────────────────────────┐  │  
│  │                          Общие модули                                │  │  
│  │  ┌──────────────┐      ┌───────────────┐      ┌────────────────────┐  │  │  
│  │  │  config.py   │◀────▶│  thermo.py    │◀────▶│ postshock.py       │  │  │  
│  │  │ (точности и  │      │ (Cantera-API) │      │ (CJ, Hugoniot,     │  │  │  
│  │  │  параметры)  │      │               │      │  постшоковые расчёты)│ │  │  
│  │  └──────────────┘      └───────────────┘      └────────────────────┘  │  │  
│  └──────────────────────────────────────────────────────────────────────┘  │  
│                                   ▲                                         │  
│                                   │                                         │  
│  ┌──────────────────────────────────────────────────────────────────────┐  │  
│  │                          Задачи (контроллеры)                         │  │  
│  │  ┌──────────────────────┐  ┌─────────────────────────┐  ┌──────────────┐  │  
│  │  │ reley_hugoniot.py    │  │ CJ_reley_hugoniot.py    │  │ CJ_Speed_*   │  │  
│  │  │ (Задача 1: заморож.) │  │ (Задача 2: равн. + CJ)  │  │ (Задача 3:    │  │  
│  │  │                      │  │                         │  │  D(u), P(u), T(u)) ││  
│  │  └──────────────────────┘  └─────────────────────────┘  └──────────────┘  │  
│  └──────────────────────────────────────────────────────────────────────┘  │  
│                                   │                                         │  
│                                   ▼                                         │  
│             ┌───────────────────────────────────────────────────────┐       │  
│             │                    YAML-механизмы                     │       │  
│             │ ┌──────────────────┐  ┌──────────────────────────────┐ │       │  
│             │ │ airNASA9ions.yaml│  │ mevel2018.yaml               │ │       │  
│             │ │ (Task 1, воздух) │  │ (Task 2, топливо)            │ │       │  
│             │ └──────────────────┘  └──────────────────────────────┘ │       │  
│             │ ┌──────────────────┐                                        │  
│             │ │ mevel2017.yaml   │                                        │  
│             │ │ (Task 3, смесь H₂/O₂) │                                    │  
│             │ └──────────────────┘                                        │  
│             └───────────────────────────────────────────────────────┘       │  
└────────────────────────────────────────────────────────────────────────────┘  
