<!doctype html>
<html lang="en">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Minimalist Planner App</title>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600&amp;display=swap" rel="stylesheet">
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Upright:wght@300;400;500&amp;display=swap" rel="stylesheet">
  <link href="https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;500;600&amp;display=swap" rel="stylesheet">
  <link href="https://fonts.googleapis.com/css2?family=Tenor+Sans&amp;display=swap" rel="stylesheet">
  <link href="https://fonts.googleapis.com/css2?family=Raleway:wght@400;500;600&amp;display=swap" rel="stylesheet">
  <style>
        body {
            box-sizing: border-box;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        html, body {
            height: 100%;
            overflow-x: hidden;
        }

        /* Theme Variables */
        :root {
            --theme-primary: #eadcdc;
            --theme-secondary: #dcc8c8;
            --theme-accent: #f0e6e6;
            --theme-category: #d4a5a5;
        }

        .theme-blue {
            --theme-primary: #c3cfde;
            --theme-secondary: #a8b8d1;
            --theme-accent: #dae4ef;
            --theme-category: #8fa3c4;
        }

        .theme-green {
            --theme-primary: #c9d4c5;
            --theme-secondary: #b0c4ab;
            --theme-accent: #d4e0d0;
            --theme-category: #9bb396;
        }

        .theme-pink {
            --theme-primary: #eadcdc;
            --theme-secondary: #dcc8c8;
            --theme-accent: #f0e6e6;
            --theme-category: #d4a5a5;
        }

        .theme-beige {
            --theme-primary: #DBD4CA;
            --theme-secondary: #cfc7bc;
            --theme-accent: #e5ddd2;
            --theme-category: #c7beb2;
        }

        body {
            font-family: 'Open Sans', sans-serif;
            background-color: #FCFBFA;
            color: #4a4a4a;
            line-height: 1.5;
        }

        .app-container {
            max-width: 800px;
            margin: 0 auto;
            background-color: #FCFBFA;
            min-height: 100vh;
            position: relative;
            padding: 0 20px;
        }

        @media (max-width: 768px) {
            .app-container {
                max-width: 100%;
                padding: 0 16px;
            }
        }

        .back-arrow {
            position: absolute;
            top: 20px;
            left: 20px;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background-color: white;
            border: 1px solid #e0e0e0;
            display: none;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.2s ease;
            z-index: 60;
            color: #4a4a4a;
        }

        .back-arrow:hover {
            background-color: var(--theme-accent);
            border-color: var(--theme-secondary);
            transform: translateX(-2px);
        }

        .back-arrow.show {
            display: flex;
        }

        .app-title {
            font-family: 'Cormorant Upright', serif;
            font-size: 30px;
            font-weight: 300;
            font-style: italic;
            color: #000;
            text-align: center;
            padding: 20px 0 10px 0;
            margin: 0;
            background-color: #FCFBFA;
            position: sticky;
            top: 0;
            z-index: 50;
        }

        .page-title {
            font-family: 'Open Sans', sans-serif;
            font-size: 24px;
            font-weight: 400;
            color: #000;
            text-align: center;
            padding: 20px 0;
            margin: 0;
        }

        .tab-content {
            display: none;
            padding: 0 0 120px 0;
        }

        .tab-content.active {
            display: block;
        }

        .calendar-strip {
            display: flex;
            gap: 8px;
            overflow-x: auto;
            padding: 12px;
            margin-bottom: 20px;
            background-color: transparent;
            scroll-snap-type: x mandatory;
            scroll-behavior: smooth;
            border-top: 1px solid #e8e8e8;
            border-bottom: 1px solid #e8e8e8;
        }

        .calendar-day {
            min-width: 50px;
            text-align: center;
            padding: 12px 8px;
            cursor: pointer;
            background-color: transparent;
            color: #4a4a4a;
            scroll-snap-align: center;
            flex-shrink: 0;
        }

        .calendar-day.active {
            background-color: var(--theme-primary);
            color: #4a4a4a;
            border-radius: 50px;
        }

        .calendar-day .day-name {
            font-size: 10px;
            color: inherit;
            margin-bottom: 4px;
            text-transform: uppercase;
            font-weight: 400;
        }

        .calendar-day .day-number {
            font-size: 16px;
            font-weight: 400;
        }

        .section {
            background-color: #ffffff;
            border-radius: 12px;
            padding: 10px;
            margin-bottom: 20px;
            border: 1px solid #e0e0e0;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.04), 0 1px 2px rgba(0, 0, 0, 0.02);
        }

        .cards-container {
            display: flex;
            gap: 12px;
            margin-bottom: 20px;
            align-items: flex-start;
        }

        .card-left, .card-right {
            flex: 1;
            margin-bottom: 0;
            width: 50%;
        }

        @media (max-width: 768px) {
            .cards-container:not(.routine-cards) {
                flex-direction: column;
                gap: 12px;
            }
            
            .cards-container:not(.routine-cards) .card-left, 
            .cards-container:not(.routine-cards) .card-right {
                margin-bottom: 0;
                width: 100%;
                flex: none;
            }
        }

        .section-header {
            font-family: 'Tenor Sans', sans-serif;
            font-size: 14px;
            font-weight: 400;
            color: #000;
            margin-bottom: 12px;
            padding-bottom: 6px;
            border-bottom: 1px solid #f0f0f0;
            text-transform: uppercase;
        }

        .section-header.no-border {
            border-bottom: none;
        }

        .progress-bar {
            width: 100%;
            height: 6px;
            background-color: #f0f0f0;
            border-radius: 3px;
            overflow: hidden;
            margin: 12px 0;
        }

        .progress-fill {
            height: 100%;
            background-color: var(--theme-secondary);
            border-radius: 3px;
            transition: width 0.3s ease;
        }

        .todo-item {
            display: flex;
            align-items: center;
            padding: 8px 0;
            border-bottom: 1px solid #f5f5f5;
            position: relative;
        }

        .todo-item:last-child {
            border-bottom: none;
        }

        .checkbox {
            width: 18px;
            height: 18px;
            border: 2px solid #000;
            border-radius: 50%;
            margin-right: 12px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.2s ease;
        }

        .checkbox.checked {
            background-color: var(--theme-secondary);
            border-color: var(--theme-secondary);
        }

        .todo-content {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 4px;
            min-width: 0;
        }

        .todo-main-row {
            display: flex;
            align-items: center;
            gap: 4px;
        }

        .todo-text {
            font-size: 14px;
            color: #000;
            cursor: pointer;
            word-wrap: break-word;
            overflow-wrap: break-word;
            hyphens: auto;
            line-height: 1.3;
        }

        .todo-text.completed {
            text-decoration: line-through;
            color: #8a8a8a;
        }

        .todo-tag {
            font-size: 10px;
            font-weight: 500;
            width: fit-content;
            text-transform: uppercase;
            color: var(--theme-category);
        }

        .priority-indicator {
            width: 8px;
            height: 8px;
            background-color: var(--theme-secondary);
            border-radius: 50%;
            margin-left: 8px;
            flex-shrink: 0;
        }

        .delete-btn {
            background: none;
            border: none;
            cursor: pointer;
            font-size: 14px;
            color: #d0d0d0;
            padding: 4px;
            margin-left: 8px;
            transition: color 0.2s ease;
        }

        .delete-btn:hover {
            color: #ff6b6b;
        }

        .add-todo-form {
            display: none;
            background-color: #f8f8f8;
            padding: 12px;
            border-radius: 4px;
            margin-top: 12px;
            border: 1px solid #e0e0e0;
        }

        .form-group {
            margin-bottom: 12px;
        }

        .form-label {
            font-size: 12px;
            color: #4a4a4a;
            margin-bottom: 4px;
            display: block;
            text-transform: uppercase;
        }

        .form-input {
            width: 100%;
            padding: 8px;
            border: 1px solid #e0e0e0;
            border-radius: 0px;
            font-size: 14px;
            background-color: white;
        }

        .form-select {
            width: 100%;
            padding: 8px;
            border: 1px solid #e0e0e0;
            border-radius: 0px;
            font-size: 14px;
            background-color: white;
        }

        .form-buttons {
            display: flex;
            gap: 8px;
            justify-content: flex-end;
        }

        .form-btn {
            padding: 6px 12px;
            border: 1px solid #e0e0e0;
            border-radius: 0px;
            font-size: 12px;
            cursor: pointer;
            background-color: white;
            color: #4a4a4a;
        }

        .form-btn.primary {
            background-color: var(--theme-primary);
            color: #4a4a4a;
        }

        .edit-input {
            background: none;
            border: none;
            font-size: 14px;
            color: #000;
            width: 100%;
            padding: 2px 0;
        }

        .edit-input:focus {
            outline: 1px solid var(--theme-primary);
            background-color: white;
            padding: 2px 4px;
        }

        .schedule-item {
            display: flex;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px solid #f5f5f5;
            position: relative;
        }

        .schedule-item:last-child {
            border-bottom: none;
        }

        .schedule-time {
            font-size: 12px;
            color: #8a8a8a;
            width: 70px;
            font-weight: 500;
            cursor: pointer;
        }

        .schedule-event {
            flex: 1;
            font-size: 14px;
            margin-left: 12px;
            cursor: pointer;
            word-wrap: break-word;
            overflow-wrap: break-word;
            hyphens: auto;
            line-height: 1.3;
            min-width: 0;
        }

        .wellness-card {
            cursor: pointer;
            transition: all 0.2s ease;
            margin-bottom: 12px;
        }

        .wellness-card:hover {
            background-color: var(--theme-accent);
            transform: translateY(-1px);
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
        }

        .wellness-cards-container {
            display: flex;
            gap: 8px;
            margin-bottom: 20px;
        }

        .wellness-cards-container .wellness-card {
            flex: 1;
            margin-bottom: 0;
        }

        @media (max-width: 768px) {
            .wellness-cards-container {
                gap: 6px;
            }
            
            .wellness-cards-container .wellness-card {
                padding: 8px;
            }
        }

        @media (max-width: 480px) {
            .wellness-cards-container {
                gap: 4px;
            }
            
            .wellness-cards-container .wellness-card {
                padding: 6px;
            }
        }

        .wellness-item {
            display: flex;
            align-items: center;
            gap: 16px;
            padding: 4px;
        }

        .wellness-item-vertical {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 12px;
            padding: 8px;
            text-align: center;
        }

        .wellness-content {
            flex: 1;
            text-align: left;
        }

        .wellness-circle {
            position: relative;
            width: 60px;
            height: 60px;
            margin-bottom: 8px;
        }

        @media (max-width: 768px) {
            .wellness-circle {
                width: 60px;
                height: 60px;
                margin-bottom: 8px;
            }
        }

        @media (max-width: 480px) {
            .wellness-circle {
                width: 60px;
                height: 60px;
                margin-bottom: 8px;
            }
        }

        .progress-ring {
            transform: rotate(-90deg);
        }

        .progress-ring-background {
            stroke: #f0f0f0;
        }

        .progress-ring-progress {
            stroke: var(--theme-secondary);
            stroke-linecap: round;
            transition: stroke-dashoffset 0.3s ease;
        }

        .wellness-icon {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            pointer-events: none;
        }

        @media (max-width: 768px) {
            .wellness-icon {
                font-size: 20px;
            }
        }

        @media (max-width: 480px) {
            .wellness-icon {
                font-size: 20px;
            }
        }

        .wellness-label {
            font-size: 12px;
            font-weight: 500;
            color: #4a4a4a;
            margin-bottom: 4px;
            text-align: center;
        }

        .wellness-value {
            font-size: 10px;
            color: #8a8a8a;
            text-align: center;
        }

        @media (max-width: 768px) {
            .wellness-label {
                font-size: 11px;
                margin-bottom: 2px;
                text-align: center;
            }
            
            .wellness-value {
                font-size: 9px;
                text-align: center;
            }
        }

        @media (max-width: 480px) {
            .wellness-label {
                font-size: 10px;
                margin-bottom: 2px;
                text-align: center;
            }
            
            .wellness-value {
                font-size: 8px;
                text-align: center;
            }
        }

        /* Wellness Goals Styles */
        .wellness-goals-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 16px;
            margin-top: 12px;
        }

        .wellness-goal-item {
            display: flex;
            align-items: center;
            gap: 16px;
            padding: 16px;
            background-color: #f8f8f8;
            border-radius: 8px;
            border: 1px solid #e0e0e0;
        }

        .wellness-goal-icon {
            font-size: 24px;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: var(--theme-accent);
            border-radius: 50%;
            flex-shrink: 0;
        }

        .wellness-goal-content {
            flex: 1;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .wellness-goal-label {
            font-size: 14px;
            font-weight: 500;
            color: #4a4a4a;
        }

        .wellness-goal-input-group {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .wellness-goal-input {
            width: 80px;
            padding: 6px 8px;
            border: 1px solid #e0e0e0;
            border-radius: 4px;
            font-size: 14px;
            text-align: center;
            background-color: white;
        }

        .wellness-goal-unit {
            font-size: 12px;
            color: #8a8a8a;
            min-width: 40px;
        }

        .habit-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
            margin-top: 12px;
        }

        .habit-item {
            background-color: #f8f8f8;
            padding: 10px;
            border-radius: 0px;
            border: 1px solid #e0e0e0;
            text-align: center;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .habit-item.completed {
            background-color: var(--theme-accent);
        }

        .habit-emoji {
            font-size: 20px;
            margin-bottom: 8px;
        }

        .habit-name {
            font-size: 12px;
            font-weight: 500;
        }

        .add-btn {
            background-color: var(--theme-primary);
            border: 1px solid #e0e0e0;
            border-radius: 16px;
            padding: 6px 12px;
            font-size: 11px;
            font-weight: 400;
            color: #4a4a4a;
            cursor: pointer;
            margin-top: 8px;
            transition: all 0.2s ease;
            width: fit-content;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 4px;
            align-self: flex-start;
        }

        .add-btn:hover {
            background-color: var(--theme-secondary);
            transform: translateY(-1px);
        }

        .vision-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 8px;
            margin-bottom: 20px;
        }

        .vision-item {
            aspect-ratio: 1;
            background-color: #f8f8f8;
            border-radius: 0px;
            border: 1px solid #e0e0e0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            cursor: pointer;
            transition: all 0.2s ease;
            position: relative;
            overflow: hidden;
        }

        .vision-item:hover {
            background-color: #f0f0f0;
        }

        .vision-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .vision-item.empty {
            border: 2px dashed #ddd;
            color: #8a8a8a;
            font-size: 18px;
        }

        .vision-item.empty:hover {
            border-color: var(--theme-secondary);
            color: var(--theme-secondary);
        }

        .vision-item-overlay {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0,0,0,0.7);
            display: none;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .vision-item:hover .vision-item-overlay {
            display: flex;
        }

        .vision-overlay-btn {
            background: white;
            border: none;
            border-radius: 4px;
            padding: 6px 8px;
            font-size: 10px;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .vision-overlay-btn:hover {
            background-color: var(--theme-accent);
        }

        .quote-box {
            background-color: var(--theme-accent);
            padding: 14px;
            border-radius: 0px;
            border: 1px solid #e0e0e0;
            text-align: center;
            font-style: italic;
            color: #4a4a4a;
        }

        .goal-item {
            display: flex;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid #f5f5f5;
            position: relative;
        }

        .goal-item:last-child {
            border-bottom: none;
        }

        .goal-content {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 8px;
            min-width: 0;
        }

        .goal-main-row {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 12px;
        }

        .goal-text {
            font-size: 14px;
            font-weight: 500;
            color: #000;
            cursor: pointer;
            word-wrap: break-word;
            overflow-wrap: break-word;
            hyphens: auto;
            line-height: 1.3;
            flex: 1;
            min-width: 0;
        }

        .goal-text.completed {
            text-decoration: line-through;
            color: #8a8a8a;
        }

        .goal-counter {
            display: flex;
            align-items: center;
            gap: 8px;
            flex-shrink: 0;
        }

        .goal-counter-btn {
            width: 24px;
            height: 24px;
            border: 1px solid #e0e0e0;
            background: white;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 14px;
            color: #4a4a4a;
            transition: all 0.2s ease;
        }

        .goal-counter-btn:hover {
            background-color: var(--theme-accent);
            border-color: var(--theme-secondary);
        }

        .goal-counter-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }

        .goal-counter-value {
            font-size: 12px;
            color: #8a8a8a;
            min-width: 40px;
            text-align: center;
        }

        .goal-progress-container {
            margin-top: 4px;
        }

        .goal-progress-bar {
            width: 100%;
            height: 4px;
            background-color: #f0f0f0;
            border-radius: 2px;
            overflow: hidden;
        }

        .goal-progress-fill {
            height: 100%;
            background-color: var(--theme-secondary);
            border-radius: 2px;
            transition: width 0.3s ease;
        }

        .goal-progress-text {
            font-size: 10px;
            color: #8a8a8a;
            margin-top: 2px;
        }

        .habit-tracker-table {
            width: 100%;
            margin-top: 16px;
        }

        .habit-row {
            display: flex;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid #f5f5f5;
        }

        .habit-label {
            width: 120px;
            font-size: 14px;
            font-weight: 500;
            word-wrap: break-word;
            overflow-wrap: break-word;
            line-height: 1.2;
        }

        .habit-days {
            display: flex;
            gap: 10px;
            flex: 1;
            align-items: center;
        }

        .habit-delete {
            background: none;
            border: none;
            cursor: pointer;
            font-size: 14px;
            color: #d0d0d0;
            padding: 4px;
            margin-left: 8px;
            transition: color 0.2s ease;
        }

        .habit-delete:hover {
            color: #ff6b6b;
        }

        .day-circle {
            width: 28px;
            height: 28px;
            border-radius: 50%;
            background-color: #f0f0f0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 10px;
            cursor: pointer;
            transition: all 0.2s ease;
            flex-shrink: 0;
        }

        .day-circle.completed {
            background-color: var(--theme-secondary);
            color: white;
        }

        .expense-table {
            width: 100%;
            margin-top: 16px;
        }

        .expense-row {
            display: grid;
            grid-template-columns: 60px 1fr 80px 60px;
            gap: 12px;
            padding: 12px 0;
            border-bottom: 1px solid #f5f5f5;
            font-size: 14px;
        }

        .expense-header {
            font-weight: 600;
            color: #8a8a8a;
            font-size: 12px;
        }

        .budget-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid #f5f5f5;
        }

        .budget-label {
            font-size: 14px;
            font-weight: 500;
        }

        .budget-amount {
            font-size: 14px;
            color: #8a8a8a;
        }

        .meal-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 8px;
            margin-top: 16px;
        }

        .meal-day {
            text-align: center;
            padding: 8px 4px;
            background-color: #f8f8f8;
            border-radius: 0px;
            border: 1px solid #e0e0e0;
            font-size: 12px;
        }

        .meal-day-name {
            font-weight: 600;
            margin-bottom: 8px;
            color: #8a8a8a;
        }

        .tab-navigation {
            position: fixed;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 100%;
            max-width: 800px;
            background-color: white;
            border-top: 1px solid #f0f0f0;
            display: flex;
            justify-content: center;
            gap: 20px;
            padding: 16px 20px 20px 20px;
            z-index: 100;
        }

        @media (max-width: 768px) {
            .tab-navigation {
                justify-content: space-around;
                gap: 0;
                padding: 16px 0 20px 0;
            }
        }

        .tab-btn {
            background: none;
            border: none;
            cursor: pointer;
            padding: 8px 16px;
            transition: all 0.3s ease;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 8px;
            position: relative;
        }

        .tab-btn svg {
            stroke: #c0c0c0;
            transition: all 0.3s ease;
        }

        .tab-btn.active svg {
            stroke: var(--theme-secondary);
        }

        .tab-btn.active::after {
            content: '';
            position: absolute;
            bottom: -12px;
            left: 50%;
            transform: translateX(-50%);
            width: 24px;
            height: 2px;
            background-color: var(--theme-secondary);
            border-radius: 1px;
            transition: all 0.3s ease;
        }

        .tab-btn:hover:not(.active) svg {
            stroke: #8a8a8a;
        }

        /* Theme Selector */
        .theme-selector {
            position: absolute;
            bottom: 80px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 100;
            display: flex;
            gap: 8px;
            background: white;
            padding: 8px;
            border-radius: 20px;
            border: 1px solid #e0e0e0;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }

        .theme-btn {
            width: 24px;
            height: 24px;
            border-radius: 50%;
            border: 2px solid transparent;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .theme-btn:hover {
            transform: scale(1.1);
        }

        .theme-btn.active {
            border-color: #4a4a4a;
        }

        .theme-btn.blue { background-color: #c3cfde; }
        .theme-btn.green { background-color: #c9d4c5; }
        .theme-btn.pink { background-color: #eadcdc; }
        .theme-btn.beige { background-color: #DBD4CA; }

        /* Planner Sub-tabs */
        .planner-tabs {
            display: flex;
            gap: 4px;
            margin-bottom: 20px;
            background-color: white;
            border-radius: 8px;
            padding: 4px;
            border: 1px solid #e0e0e0;
        }

        .planner-tab-btn {
            flex: 1;
            background: white;
            border: none;
            font-size: 14px;
            color: #8a8a8a;
            cursor: pointer;
            padding: 10px 16px;
            border-radius: 6px;
            transition: all 0.2s ease;
            font-weight: 500;
        }

        .planner-tab-btn.active {
            background-color: var(--theme-primary);
            color: #4a4a4a;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
        }

        .planner-content {
            display: none;
        }

        .planner-content.active {
            display: block;
        }

        /* Time Blocks Styles */
        .time-blocks-container {
            display: flex;
            flex-direction: column;
            gap: 16px;
        }

        .time-block {
            display: flex;
            align-items: flex-start;
            gap: 12px;
            padding: 12px;
            background-color: #f8f8f8;
            border-radius: 8px;
            border: 1px solid #e0e0e0;
            transition: all 0.2s ease;
        }

        .time-block:hover {
            background-color: #f0f0f0;
        }

        .time-block-icon {
            color: var(--theme-secondary);
            flex-shrink: 0;
            margin-top: 2px;
        }

        .time-block-content {
            flex: 1;
            min-width: 0;
        }

        .time-block-label {
            font-size: 12px;
            font-weight: 500;
            color: #8a8a8a;
            margin-bottom: 8px;
            text-transform: uppercase;
        }

        .time-block-plans {
            display: flex;
            flex-direction: column;
            gap: 8px;
            margin-bottom: 8px;
        }

        .time-block-plan {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 6px 0;
            border-bottom: 1px solid #f5f5f5;
        }

        .time-block-plan:last-child {
            border-bottom: none;
        }

        .plan-checkbox {
            width: 16px;
            height: 16px;
            border: 2px solid #ddd;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.2s ease;
            flex-shrink: 0;
        }

        .plan-checkbox.checked {
            background-color: var(--theme-secondary);
            border-color: var(--theme-secondary);
        }

        .plan-text {
            flex: 1;
            font-size: 14px;
            color: #4a4a4a;
            cursor: pointer;
            line-height: 1.3;
            word-wrap: break-word;
            overflow-wrap: break-word;
            min-width: 0;
        }

        .plan-text.completed {
            text-decoration: line-through;
            color: #8a8a8a;
        }

        .plan-delete {
            background: none;
            border: none;
            cursor: pointer;
            font-size: 12px;
            color: #d0d0d0;
            padding: 2px;
            transition: color 0.2s ease;
            flex-shrink: 0;
        }

        .plan-delete:hover {
            color: #ff6b6b;
        }

        .add-plan-btn {
            background: none;
            border: none;
            color: var(--theme-secondary);
            cursor: pointer;
            font-size: 12px;
            padding: 4px 0;
            text-align: left;
            transition: color 0.2s ease;
        }

        .add-plan-btn:hover {
            color: var(--theme-category);
        }

        /* Custom Modal Styles */
        .custom-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 2000;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .modal-dialog {
            background: white;
            padding: 24px;
            border-radius: 8px;
            max-width: 300px;
            width: 90%;
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
        }

        .modal-message {
            margin-bottom: 16px;
            font-size: 14px;
            color: #4a4a4a;
        }

        .modal-input {
            width: 100%;
            padding: 8px;
            border: 1px solid #e0e0e0;
            border-radius: 4px;
            margin-bottom: 16px;
            font-size: 14px;
        }

        .modal-buttons {
            display: flex;
            gap: 8px;
            justify-content: flex-end;
        }

        .modal-btn {
            padding: 8px 16px;
            border: 1px solid #e0e0e0;
            background: white;
            border-radius: 4px;
            cursor: pointer;
            font-size: 12px;
            transition: all 0.2s ease;
        }

        .modal-btn.primary {
            background: var(--theme-primary);
        }

        .modal-btn:hover {
            background-color: #f0f0f0;
        }

        .modal-btn.primary:hover {
            background-color: var(--theme-secondary);
        }

        /* Toast Styles */
        .toast {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: var(--theme-secondary);
            color: white;
            padding: 16px 24px;
            border-radius: 8px;
            z-index: 1000;
            font-size: 14px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
        }

        /* Circular Calendar Styles */
        .calendar-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
        }

        .calendar-header {
            display: flex;
            align-items: center;
            gap: 20px;
            margin-bottom: 10px;
        }

        .calendar-nav-btn {
            background: none;
            border: 2px solid #e0e0e0;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 18px;
            color: #4a4a4a;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.2s ease;
        }

        .calendar-nav-btn:hover {
            background-color: var(--theme-accent);
            border-color: var(--theme-secondary);
        }

        .calendar-month-year {
            font-family: 'Tenor Sans', sans-serif;
            font-size: 18px;
            font-weight: 500;
            color: #4a4a4a;
            min-width: 150px;
            text-align: center;
        }

        .circular-calendar {
            width: 100%;
            max-width: 380px;
            aspect-ratio: 1;
            border: 1px solid #e0e0e0;
            border-radius: 50%;
            background-color: white;
            padding: 15px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08), 0 2px 4px rgba(0, 0, 0, 0.04);
            position: relative;
            margin: 0 auto;
        }

        @media (max-width: 768px) {
            .circular-calendar {
                max-width: 280px;
                padding: 12px;
            }
        }

        @media (max-width: 480px) {
            .circular-calendar {
                max-width: 240px;
                padding: 10px;
            }
        }

        .calendar-month-display {
            font-family: 'Tenor Sans', sans-serif;
            font-size: 14px;
            font-weight: 500;
            color: #8a8a8a;
            margin-bottom: 16px;
            text-align: center;
        }

        @media (max-width: 480px) {
            .calendar-month-display {
                font-size: 12px;
                margin-bottom: 12px;
            }
        }

        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 3px;
            width: 100%;
            max-width: 250px;
        }

        @media (max-width: 768px) {
            .calendar-grid {
                max-width: 200px;
                gap: 2px;
            }
        }

        @media (max-width: 480px) {
            .calendar-grid {
                max-width: 170px;
                gap: 1px;
            }
        }

        .calendar-day-header {
            text-align: center;
            font-size: 10px;
            font-weight: 600;
            color: #8a8a8a;
            padding: 4px;
            text-transform: uppercase;
        }

        .calendar-day-circle {
            width: 28px;
            height: 28px;
            border-radius: 50%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            font-size: 11px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s ease;
            background-color: transparent;
            border: none;
            color: #4a4a4a;
            margin: 0 auto;
            position: relative;
        }

        @media (max-width: 768px) {
            .calendar-day-circle {
                width: 24px;
                height: 24px;
                font-size: 10px;
            }
        }

        @media (max-width: 480px) {
            .calendar-day-circle {
                width: 20px;
                height: 20px;
                font-size: 9px;
            }
        }

        .calendar-day-circle:hover {
            background-color: var(--theme-accent);
            transform: scale(1.05);
        }

        .calendar-day-circle.today {
            background-color: var(--theme-primary);
            color: #4a4a4a;
            font-weight: 600;
            border: 2px solid var(--theme-secondary);
        }

        .calendar-day-circle.has-tasks {
            position: relative;
        }

        .calendar-day-circle.has-tasks::after {
            content: '';
            position: absolute;
            bottom: 2px;
            left: 50%;
            transform: translateX(-50%);
            width: 6px;
            height: 6px;
            background-color: var(--theme-secondary);
            border-radius: 50%;
        }

        .calendar-day-circle.other-month {
            color: #d0d0d0;
            background-color: #f8f8f8;
        }

        .calendar-day-circle.other-month:hover {
            background-color: #f0f0f0;
        }

        /* Weekly Calendar Styles */
        .weekly-calendar-header {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 20px;
            margin-bottom: 20px;
        }

        .weekly-date-range {
            font-family: 'Open Sans', sans-serif;
            font-size: 16px;
            font-weight: 500;
            color: #4a4a4a;
            min-width: 180px;
            text-align: center;
        }

        .weekly-calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 8px;
            margin-bottom: 20px;
        }

        @media (max-width: 768px) {
            .weekly-calendar-grid {
                grid-template-columns: 1fr;
                gap: 8px;
            }
        }

        @media (max-width: 480px) {
            .weekly-calendar-grid {
                gap: 6px;
            }
        }

        .weekly-day-card {
            background-color: #FCFBFA;
            border: 1px solid #e0e0e0;
            border-radius: 8px;
            padding: 12px 8px;
            min-height: 200px;
            display: flex;
            flex-direction: column;
            transition: all 0.2s ease;
        }

        @media (max-width: 768px) {
            .weekly-day-card {
                min-height: auto;
                padding: 12px;
                flex-direction: row;
                align-items: flex-start;
                gap: 12px;
            }
        }

        @media (max-width: 480px) {
            .weekly-day-card {
                padding: 10px;
                gap: 10px;
            }
        }

        .weekly-day-card.today {
            background-color: var(--theme-accent);
            border-color: var(--theme-secondary);
        }

        .weekly-day-header {
            text-align: center;
            margin-bottom: 12px;
            padding-bottom: 8px;
            border-bottom: 1px solid #e8e8e8;
        }

        @media (max-width: 768px) {
            .weekly-day-header {
                text-align: left;
                margin-bottom: 0;
                padding-bottom: 0;
                border-bottom: none;
                min-width: 60px;
                flex-shrink: 0;
            }
        }

        .weekly-day-name {
            font-size: 12px;
            font-weight: 600;
            color: #4a4a4a;
            text-transform: uppercase;
            margin-bottom: 4px;
        }

        .weekly-day-number {
            font-size: 16px;
            font-weight: 500;
            color: #4a4a4a;
        }

        .weekly-day-tasks {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        @media (max-width: 768px) {
            .weekly-day-tasks {
                min-width: 0;
            }
        }

        .weekly-task-item {
            background-color: white;
            border: 1px solid #e8e8e8;
            border-radius: 4px;
            padding: 6px 8px;
            font-size: 12px;
            color: #4a4a4a;
            cursor: pointer;
            transition: all 0.2s ease;
            display: flex;
            align-items: center;
            gap: 6px;
            line-height: 1.2;
        }

        @media (max-width: 768px) {
            .weekly-task-item {
                padding: 8px;
                font-size: 13px;
                gap: 8px;
            }
        }

        .weekly-task-item:hover {
            background-color: var(--theme-accent);
            border-color: var(--theme-secondary);
        }

        .weekly-task-checkbox {
            width: 12px;
            height: 12px;
            border: 1px solid #ddd;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.2s ease;
            flex-shrink: 0;
        }

        .weekly-task-checkbox.checked {
            background-color: var(--theme-secondary);
            border-color: var(--theme-secondary);
        }

        .weekly-task-text {
            flex: 1;
            word-wrap: break-word;
            overflow-wrap: break-word;
            min-width: 0;
        }

        .weekly-task-text.completed {
            text-decoration: line-through;
            color: #8a8a8a;
        }

        .weekly-task-delete {
            background: none;
            border: none;
            cursor: pointer;
            font-size: 10px;
            color: #d0d0d0;
            padding: 2px;
            transition: color 0.2s ease;
            flex-shrink: 0;
        }

        .weekly-task-delete:hover {
            color: #ff6b6b;
        }

        .weekly-add-task {
            background: none;
            border: 1px dashed #ddd;
            border-radius: 4px;
            padding: 6px;
            font-size: 11px;
            color: #8a8a8a;
            cursor: pointer;
            text-align: center;
            transition: all 0.2s ease;
            margin-top: 4px;
        }

        @media (max-width: 768px) {
            .weekly-add-task {
                padding: 8px;
                font-size: 12px;
                margin-top: 8px;
            }
        }

        .weekly-add-task:hover {
            border-color: var(--theme-secondary);
            color: var(--theme-secondary);
            background-color: var(--theme-accent);
        }

        /* Task Popup Styles */
        .task-popup {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 2000;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .task-popup-content {
            background: white;
            padding: 24px;
            border-radius: 12px;
            max-width: 400px;
            width: 90%;
            max-height: 80vh;
            overflow-y: auto;
            box-shadow: 0 4px 20px rgba(0,0,0,0.15);
        }

        .task-popup-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 12px;
            border-bottom: 1px solid #f0f0f0;
        }

        .task-popup-title {
            font-family: 'Tenor Sans', sans-serif;
            font-size: 16px;
            font-weight: 500;
            color: #4a4a4a;
        }

        .task-popup-close {
            background: none;
            border: none;
            font-size: 20px;
            cursor: pointer;
            color: #8a8a8a;
            padding: 4px;
        }

        .task-popup-close:hover {
            color: #4a4a4a;
        }

        .task-popup-list {
            margin-bottom: 20px;
        }

        .task-popup-item {
            display: flex;
            align-items: center;
            padding: 8px 0;
            border-bottom: 1px solid #f5f5f5;
        }

        .task-popup-item:last-child {
            border-bottom: none;
        }

        .task-popup-checkbox {
            width: 16px;
            height: 16px;
            border: 2px solid #ddd;
            border-radius: 50%;
            margin-right: 12px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.2s ease;
            flex-shrink: 0;
        }

        .task-popup-checkbox.checked {
            background-color: var(--theme-secondary);
            border-color: var(--theme-secondary);
        }

        .task-popup-text {
            flex: 1;
            font-size: 14px;
            color: #4a4a4a;
            cursor: pointer;
            line-height: 1.3;
        }

        .task-popup-text.completed {
            text-decoration: line-through;
            color: #8a8a8a;
        }

        .task-popup-delete {
            background: none;
            border: none;
            cursor: pointer;
            font-size: 14px;
            color: #d0d0d0;
            padding: 4px;
            margin-left: 8px;
            transition: color 0.2s ease;
            flex-shrink: 0;
        }

        .task-popup-delete:hover {
            color: #ff6b6b;
        }

        .task-popup-add-form {
            display: none;
            background-color: #f8f8f8;
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 12px;
            border: 1px solid #e0e0e0;
        }

        .task-popup-input {
            width: 100%;
            padding: 8px;
            border: 1px solid #e0e0e0;
            border-radius: 4px;
            font-size: 14px;
            background-color: white;
            margin-bottom: 8px;
        }

        .task-popup-buttons {
            display: flex;
            gap: 8px;
            justify-content: flex-end;
        }

        .task-popup-btn {
            padding: 6px 12px;
            border: 1px solid #e0e0e0;
            border-radius: 4px;
            font-size: 12px;
            cursor: pointer;
            background-color: white;
            transition: all 0.2s ease;
        }

        .task-popup-btn.primary {
            background-color: var(--theme-primary);
        }

        .task-popup-btn:hover {
            background-color: #f0f0f0;
        }

        .task-popup-btn.primary:hover {
            background-color: var(--theme-secondary);
        }

        /* Routine Card Styles */
        .routine-header {
            display: flex;
            align-items: flex-start;
            gap: 12px;
            margin-bottom: 16px;
        }

        .routine-icon {
            flex-shrink: 0;
            margin-top: 2px;
        }

        .morning-icon {
            color: var(--theme-secondary);
        }

        .night-icon {
            color: var(--theme-secondary);
        }

        .routine-title-section {
            flex: 1;
            min-width: 0;
        }

        .routine-time {
            font-size: 12px;
            color: #8a8a8a;
            margin-top: 4px;
        }
    </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
  <script src="/_sdk/element_sdk.js" type="text/javascript"></script>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body class="theme-pink">
  <div class="app-container"><!-- Theme Selector -->
   <div class="theme-selector">
    <div class="theme-btn blue" onclick="changeTheme('blue')" title="Blue Theme"></div>
    <div class="theme-btn green" onclick="changeTheme('green')" title="Green Theme"></div>
    <div class="theme-btn pink active" onclick="changeTheme('pink')" title="Pink Theme"></div>
    <div class="theme-btn beige" onclick="changeTheme('beige')" title="Beige Theme"></div>
   </div><!-- Back Arrow -->
   <div class="back-arrow" id="backArrow" onclick="goBackToHome()">
    <svg width="20" height="20" viewbox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M19 12H5" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" /> <path d="M12 19l-7-7 7-7" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
    </svg>
   </div>
   <div class="app-title" id="mainTitle">
    Pillars of Self
   </div><!-- Home Tab -->
   <div class="tab-content active" id="home"><!-- Calendar Strip -->
    <div class="calendar-strip" id="calendarStrip"><!-- Calendar days will be generated by JavaScript -->
    </div><!-- Schedule and To-Do List Side by Side -->
    <div class="cards-container"><!-- To-Do List -->
     <div class="section card-left">
      <div class="section-header">
       TO DO
      </div>
      <div class="progress-bar">
       <div class="progress-fill" id="todoProgress" style="width: 0%"></div>
      </div>
      <div id="todoList"><!-- Tasks will be loaded here based on selected date -->
      </div>
      <div class="add-todo-form" id="addTodoForm">
       <div class="form-group"><label class="form-label" for="taskInput">Task</label> <input type="text" id="taskInput" class="form-input" placeholder="Enter task...">
       </div>
       <div class="form-group"><label class="form-label" for="tagSelect">Category</label> <select id="tagSelect" class="form-select"> <option value="">No Category</option> <option value="work">Work</option> <option value="selfcare">Self Care</option> <option value="personal">Personal</option> <option value="home">Home</option> </select>
       </div>
       <div class="form-group"><label class="form-label"> <input type="checkbox" id="priorityCheckbox" style="margin-right: 6px;"> Mark as Priority </label>
       </div>
       <div class="form-buttons"><button class="form-btn" onclick="cancelAddTodo()">Cancel</button> <button class="form-btn primary" onclick="saveTodo()">Add Task</button>
       </div>
      </div><button class="add-btn" onclick="showAddTodoForm()"> <span style="font-size: 14px;">+</span> Add task </button>
     </div><!-- Schedule -->
     <div class="section card-right">
      <div class="section-header">
       SCHEDULE
      </div>
      <div id="scheduleList"><!-- Schedule items will be loaded here based on selected date -->
      </div>
      <div class="add-todo-form" id="addScheduleForm">
       <div class="form-group"><label class="form-label" for="timeInput">Time</label> <input type="time" id="timeInput" class="form-input">
       </div>
       <div class="form-group"><label class="form-label" for="eventInput">Event</label> <input type="text" id="eventInput" class="form-input" placeholder="Enter event...">
       </div>
       <div class="form-buttons"><button class="form-btn" onclick="cancelAddSchedule()">Cancel</button> <button class="form-btn primary" onclick="saveSchedule()">Add Event</button>
       </div>
      </div><button class="add-btn" onclick="showAddScheduleForm()"> <span style="font-size: 14px;">+</span> Add event </button>
     </div>
    </div><!-- Wellness Tracker -->
    <div class="section-header" style="margin-bottom: 16px; padding: 0 10px;">
     Wellness
    </div>
    <div class="wellness-cards-container"><!-- Water Card -->
     <div class="section wellness-card" onclick="updateWellnessItem('water')">
      <div class="wellness-item-vertical">
       <div class="wellness-circle" id="waterCircle">
        <svg class="progress-ring" width="60" height="60"><circle class="progress-ring-background" cx="30" cy="30" r="25" stroke="#f0f0f0" stroke-width="4" fill="transparent" /> <circle class="progress-ring-progress" id="waterProgress" cx="30" cy="30" r="25" stroke="var(--theme-secondary)" stroke-width="4" fill="transparent" stroke-dasharray="157.08" stroke-dashoffset="157.08" />
        </svg>
        <div class="wellness-icon">
         <svg width="20" height="20" viewbox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M12 2.5c-3.5 4-8 7.5-8 12a8 8 0 0 0 16 0c0-4.5-4.5-8-8-12z" stroke="var(--theme-secondary)" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" fill="none" /> <path d="M8 14c0-2 2-4 4-4" stroke="var(--theme-secondary)" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" fill="none" />
         </svg>
        </div>
       </div>
       <div class="wellness-content">
        <div class="wellness-label">
         Water
        </div>
        <div class="wellness-value" id="waterValue">
         0/8 cups
        </div>
       </div>
      </div>
     </div><!-- Steps Card -->
     <div class="section wellness-card" onclick="updateWellnessItem('steps')">
      <div class="wellness-item-vertical">
       <div class="wellness-circle" id="stepsCircle">
        <svg class="progress-ring" width="60" height="60"><circle class="progress-ring-background" cx="30" cy="30" r="25" stroke="#f0f0f0" stroke-width="4" fill="transparent" /> <circle class="progress-ring-progress" id="stepsProgress" cx="30" cy="30" r="25" stroke="var(--theme-secondary)" stroke-width="4" fill="transparent" stroke-dasharray="157.08" stroke-dashoffset="157.08" />
        </svg>
        <div class="wellness-icon">
         <svg width="20" height="20" viewbox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><!-- Left footprint --> <ellipse cx="8" cy="16" rx="2.5" ry="4" fill="var(--theme-secondary)" opacity="0.8" /> <circle cx="7" cy="12" r="0.8" fill="var(--theme-secondary)" /> <circle cx="8.5" cy="11.5" r="0.6" fill="var(--theme-secondary)" /> <circle cx="9.5" cy="12" r="0.5" fill="var(--theme-secondary)" /> <!-- Right footprint --> <ellipse cx="16" cy="10" rx="2.5" ry="4" fill="var(--theme-secondary)" opacity="0.6" /> <circle cx="15" cy="6" r="0.8" fill="var(--theme-secondary)" opacity="0.6" /> <circle cx="16.5" cy="5.5" r="0.6" fill="var(--theme-secondary)" opacity="0.6" /> <circle cx="17.5" cy="6" r="0.5" fill="var(--theme-secondary)" opacity="0.6" />
         </svg>
        </div>
       </div>
       <div class="wellness-content">
        <div class="wellness-label">
         Steps
        </div>
        <div class="wellness-value" id="stepsValue">
         0/10k steps
        </div>
       </div>
      </div>
     </div><!-- Sleep Card -->
     <div class="section wellness-card" onclick="updateWellnessItem('sleep')">
      <div class="wellness-item-vertical">
       <div class="wellness-circle" id="sleepCircle">
        <svg class="progress-ring" width="60" height="60"><circle class="progress-ring-background" cx="30" cy="30" r="25" stroke="#f0f0f0" stroke-width="4" fill="transparent" /> <circle class="progress-ring-progress" id="sleepProgress" cx="30" cy="30" r="25" stroke="var(--theme-secondary)" stroke-width="4" fill="transparent" stroke-dasharray="157.08" stroke-dashoffset="157.08" />
        </svg>
        <div class="wellness-icon">
         <svg width="20" height="20" viewbox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z" stroke="var(--theme-secondary)" stroke-width="1.5" fill="none" /> <path d="M16 8l-1 1-1-1M14 12l-1 1-1-1" stroke="var(--theme-secondary)" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" />
         </svg>
        </div>
       </div>
       <div class="wellness-content">
        <div class="wellness-label">
         Sleep
        </div>
        <div class="wellness-value" id="sleepValue">
         0/8 hours
        </div>
       </div>
      </div>
     </div>
    </div>
   </div><!-- Planner Tab -->
   <div class="tab-content" id="planner">
    <div class="page-title">
     Planner
    </div><!-- Planner Sub-tabs -->
    <div class="planner-tabs"><button class="planner-tab-btn active" onclick="switchPlannerTab('daily')">Daily</button> <button class="planner-tab-btn" onclick="switchPlannerTab('weekly')">Weekly</button> <button class="planner-tab-btn" onclick="switchPlannerTab('monthly')">Monthly</button>
    </div><!-- Daily Planner -->
    <div class="planner-content active" id="daily-planner">
     <div class="section">
      <div class="section-header">
       Daily Priorities
      </div>
      <div id="dailyPrioritiesList"><!-- User's daily priorities will be loaded here -->
      </div>
      <div class="add-todo-form" id="addDailyPriorityForm">
       <div class="form-group"><label class="form-label" for="dailyPriorityInput">Priority</label> <input type="text" id="dailyPriorityInput" class="form-input" placeholder="Enter priority...">
       </div>
       <div class="form-buttons"><button class="form-btn" onclick="cancelAddDailyPriority()">Cancel</button> <button class="form-btn primary" onclick="saveDailyPriority()">Add Priority</button>
       </div>
      </div><button class="add-btn" onclick="showAddDailyPriorityForm()">+ Add Priority</button>
     </div>
     <div class="section">
      <div class="section-header no-border">
       TIME BLOCKS
      </div>
      <div class="time-blocks-container">
       <div class="time-block">
        <div class="time-block-icon">
         <svg width="16" height="16" viewbox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><circle cx="12" cy="12" r="5" stroke="currentColor" stroke-width="2" /> <line x1="12" y1="1" x2="12" y2="3" stroke="currentColor" stroke-width="2" /> <line x1="12" y1="21" x2="12" y2="23" stroke="currentColor" stroke-width="2" /> <line x1="4.22" y1="4.22" x2="5.64" y2="5.64" stroke="currentColor" stroke-width="2" /> <line x1="18.36" y1="18.36" x2="19.78" y2="19.78" stroke="currentColor" stroke-width="2" /> <line x1="1" y1="12" x2="3" y2="12" stroke="currentColor" stroke-width="2" /> <line x1="21" y1="12" x2="23" y2="12" stroke="currentColor" stroke-width="2" />
         </svg>
        </div>
        <div class="time-block-content">
         <div class="time-block-label">
          Morning
         </div>
         <div id="morning-plans" class="time-block-plans"><!-- Morning plans will be loaded here -->
         </div><button class="add-plan-btn" onclick="addMorningPlan()">+ Add plan</button>
        </div>
       </div>
       <div class="time-block">
        <div class="time-block-icon">
         <svg width="16" height="16" viewbox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><circle cx="12" cy="12" r="5" stroke="currentColor" stroke-width="2" /> <line x1="12" y1="7" x2="12" y2="13" stroke="currentColor" stroke-width="2" /> <line x1="12" y1="13" x2="15" y2="15" stroke="currentColor" stroke-width="2" />
         </svg>
        </div>
        <div class="time-block-content">
         <div class="time-block-label">
          Afternoon
         </div>
         <div id="afternoon-plans" class="time-block-plans"><!-- Afternoon plans will be loaded here -->
         </div><button class="add-plan-btn" onclick="addAfternoonPlan()">+ Add plan</button>
        </div>
       </div>
       <div class="time-block">
        <div class="time-block-icon">
         <svg width="16" height="16" viewbox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z" stroke="currentColor" stroke-width="2" fill="none" />
         </svg>
        </div>
        <div class="time-block-content">
         <div class="time-block-label">
          Evening
         </div>
         <div id="evening-plans" class="time-block-plans"><!-- Evening plans will be loaded here -->
         </div><button class="add-plan-btn" onclick="addEveningPlan()">+ Add plan</button>
        </div>
       </div>
      </div>
     </div>
    </div><!-- Weekly Planner -->
    <div class="planner-content" id="weekly-planner">
     <div class="section">
      <div class="section-header">
       Weekly Planner
      </div>
      <div class="weekly-calendar-header"><button class="calendar-nav-btn" onclick="previousWeek()">‹</button>
       <div class="weekly-date-range" id="weeklyDateRange">
        Dec 16 - Dec 22, 2024
       </div><button class="calendar-nav-btn" onclick="nextWeek()">›</button>
      </div>
      <div class="weekly-calendar-grid" id="weeklyCalendarGrid"><!-- Weekly calendar will be generated here -->
      </div>
     </div>
    </div><!-- Monthly Planner -->
    <div class="planner-content" id="monthly-planner">
     <div class="calendar-container">
      <div class="calendar-header"><button class="calendar-nav-btn" onclick="previousMonth()">‹</button>
       <div class="calendar-month-year" id="calendarMonthYear">
        December 2024
       </div><button class="calendar-nav-btn" onclick="nextMonth()">›</button>
      </div>
      <div class="circular-calendar" id="circularCalendar"><!-- Calendar days will be generated here -->
      </div>
     </div>
    </div>
   </div><!-- Goals Tab -->
   <div class="tab-content" id="goals">
    <div class="page-title">
     Goals
    </div>
    <div class="section">
     <div class="section-header">
      Vision Board
     </div>
     <div class="vision-grid" id="visionGrid"><!-- Vision board items will be loaded here -->
     </div><input type="file" id="visionImageInput" accept="image/*" style="display: none;">
    </div>
    <div class="section">
     <div class="section-header">
      Goals Tracker
     </div>
     <div id="goalsList"><!-- Goals will be loaded here -->
     </div>
     <div class="add-todo-form" id="addGoalForm">
      <div class="form-group"><label class="form-label" for="goalInput">Goal</label> <input type="text" id="goalInput" class="form-input" placeholder="Enter your goal...">
      </div>
      <div class="form-group"><label class="form-label" for="goalTargetInput">Target (optional)</label> <input type="number" id="goalTargetInput" class="form-input" placeholder="e.g., 10 for '10 books'">
      </div>
      <div class="form-buttons"><button class="form-btn" onclick="cancelAddGoal()">Cancel</button> <button class="form-btn primary" onclick="saveGoal()">Add Goal</button>
      </div>
     </div><button class="add-btn" onclick="showAddGoalForm()"> <span style="font-size: 14px;">+</span> Add goal </button>
    </div>
    <div class="section">
     <div class="section-header">
      Today's Intention
     </div>
     <div class="quote-box">
      "Focus on progress, not perfection. Every small step counts toward your bigger dreams."
     </div>
    </div>
   </div><!-- Habits Tab -->
   <div class="tab-content" id="habits">
    <div class="page-title">
     Habits
    </div><!-- Wellness Goals Section -->
    <div class="section">
     <div class="section-header">
      Wellness Goals
     </div>
     <div class="wellness-goals-grid">
      <div class="wellness-goal-item">
       <div class="wellness-goal-icon">
        💧
       </div>
       <div class="wellness-goal-content">
        <div class="wellness-goal-label">
         Water Goal
        </div>
        <div class="wellness-goal-input-group"><input type="number" id="waterGoal" class="wellness-goal-input" value="8" min="1" max="20" onchange="updateWellnessGoal('water', this.value)"> <span class="wellness-goal-unit">cups</span>
        </div>
       </div>
      </div>
      <div class="wellness-goal-item">
       <div class="wellness-goal-icon">
        👟
       </div>
       <div class="wellness-goal-content">
        <div class="wellness-goal-label">
         Steps Goal
        </div>
        <div class="wellness-goal-input-group"><input type="number" id="stepsGoal" class="wellness-goal-input" value="10000" min="1000" max="50000" step="1000" onchange="updateWellnessGoal('steps', this.value)"> <span class="wellness-goal-unit">steps</span>
        </div>
       </div>
      </div>
      <div class="wellness-goal-item">
       <div class="wellness-goal-icon">
        😴
       </div>
       <div class="wellness-goal-content">
        <div class="wellness-goal-label">
         Sleep Goal
        </div>
        <div class="wellness-goal-input-group"><input type="number" id="sleepGoal" class="wellness-goal-input" value="8" min="4" max="12" step="0.5" onchange="updateWellnessGoal('sleep', this.value)"> <span class="wellness-goal-unit">hours</span>
        </div>
       </div>
      </div>
     </div>
    </div><!-- Custom Habits Section -->
    <div class="section">
     <div class="section-header">
      Custom Habits
     </div>
     <div class="habit-tracker-table" id="habitTrackerTable"><!-- Habits will be loaded here -->
     </div>
     <div class="add-todo-form" id="addHabitForm">
      <div class="form-group"><label class="form-label" for="habitNameInput">Habit Name</label> <input type="text" id="habitNameInput" class="form-input" placeholder="Enter habit name...">
      </div>
      <div class="form-buttons"><button class="form-btn" onclick="cancelAddHabit()">Cancel</button> <button class="form-btn primary" onclick="saveHabit()">Add Habit</button>
      </div>
     </div><button class="add-btn" onclick="showAddHabitForm()"> <span style="font-size: 14px;">+</span> Add habit </button>
    </div><!-- Routine Cards Container -->
    <div class="cards-container routine-cards"><!-- Morning Routine Card -->
     <div class="section card-left">
      <div class="routine-header">
       <div class="routine-icon morning-icon">
        <svg width="20" height="20" viewbox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><circle cx="12" cy="12" r="5" stroke="currentColor" stroke-width="2" /> <line x1="12" y1="1" x2="12" y2="3" stroke="currentColor" stroke-width="2" /> <line x1="12" y1="21" x2="12" y2="23" stroke="currentColor" stroke-width="2" /> <line x1="4.22" y1="4.22" x2="5.64" y2="5.64" stroke="currentColor" stroke-width="2" /> <line x1="18.36" y1="18.36" x2="19.78" y2="19.78" stroke="currentColor" stroke-width="2" /> <line x1="1" y1="12" x2="3" y2="12" stroke="currentColor" stroke-width="2" /> <line x1="21" y1="12" x2="23" y2="12" stroke="currentColor" stroke-width="2" /> <line x1="4.22" y1="19.78" x2="5.64" y2="18.36" stroke="currentColor" stroke-width="2" /> <line x1="18.36" y1="5.64" x2="19.78" y2="4.22" stroke="currentColor" stroke-width="2" />
        </svg>
       </div>
       <div class="routine-title-section">
        <div class="section-header no-border">
         Morning Routine
        </div>
        <div class="routine-time"><span id="morningTime" onclick="editRoutineTime('morning')" style="cursor: pointer;">7:00 AM</span>
        </div>
       </div>
      </div>
      <div id="morningRoutineList"><!-- Morning routine items will be loaded here -->
      </div>
      <div class="add-todo-form" id="addMorningRoutineForm">
       <div class="form-group"><label class="form-label" for="morningRoutineInput">Routine Item</label> <input type="text" id="morningRoutineInput" class="form-input" placeholder="Enter routine item...">
       </div>
       <div class="form-buttons"><button class="form-btn" onclick="cancelAddMorningRoutine()">Cancel</button> <button class="form-btn primary" onclick="saveMorningRoutine()">Add Item</button>
       </div>
      </div><button class="add-btn" onclick="showAddMorningRoutineForm()"> <span style="font-size: 14px;">+</span> Add routine item </button>
     </div><!-- Night Routine Card -->
     <div class="section card-right">
      <div class="routine-header">
       <div class="routine-icon night-icon">
        <svg width="20" height="20" viewbox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z" stroke="currentColor" stroke-width="2" fill="none" />
        </svg>
       </div>
       <div class="routine-title-section">
        <div class="section-header no-border">
         Night Routine
        </div>
        <div class="routine-time"><span id="nightTime" onclick="editRoutineTime('night')" style="cursor: pointer;">10:00 PM</span>
        </div>
       </div>
      </div>
      <div id="nightRoutineList"><!-- Night routine items will be loaded here -->
      </div>
      <div class="add-todo-form" id="addNightRoutineForm">
       <div class="form-group"><label class="form-label" for="nightRoutineInput">Routine Item</label> <input type="text" id="nightRoutineInput" class="form-input" placeholder="Enter routine item...">
       </div>
       <div class="form-buttons"><button class="form-btn" onclick="cancelAddNightRoutine()">Cancel</button> <button class="form-btn primary" onclick="saveNightRoutine()">Add Item</button>
       </div>
      </div><button class="add-btn" onclick="showAddNightRoutineForm()"> <span style="font-size: 14px;">+</span> Add routine item </button>
     </div>
    </div>
   </div><!-- Finances Tab -->
   <div class="tab-content" id="finances">
    <div class="page-title">
     Finances
    </div>
    <div class="section">
     <div class="section-header">
      Expense Tracker
     </div>
     <div class="expense-table">
      <div class="expense-row">
       <div class="expense-header">
        Date
       </div>
       <div class="expense-header">
        Item
       </div>
       <div class="expense-header">
        Category
       </div>
       <div class="expense-header">
        Amount
       </div>
      </div>
      <div class="expense-row">
       <div>
        12/15
       </div>
       <div>
        Coffee
       </div>
       <div>
        Food
       </div>
       <div>
        $4.50
       </div>
      </div>
      <div class="expense-row">
       <div>
        12/15
       </div>
       <div>
        Gas
       </div>
       <div>
        Transport
       </div>
       <div>
        $45.00
       </div>
      </div>
     </div><button class="add-btn" onclick="showAddExpenseForm()">+ Add Expense</button>
    </div>
    <div class="section">
     <div class="section-header">
      Budget Overview
     </div>
     <div class="budget-item">
      <div class="budget-label">
       Food
      </div>
      <div class="budget-amount">
       $320 / $400
      </div>
     </div>
     <div class="progress-bar">
      <div class="progress-fill" style="width: 80%"></div>
     </div>
    </div>
   </div><!-- Lifestyle Tab -->
   <div class="tab-content" id="lifestyle">
    <div class="page-title">
     Lifestyle
    </div>
    <div class="section">
     <div class="section-header">
      Cleaning Checklist
     </div>
     <div class="todo-item">
      <div class="checkbox checked" onclick="toggleTodo(this)"></div>
      <div class="todo-content">
       <div class="todo-main-row">
        <div class="todo-text completed">
         Make beds
        </div>
       </div>
      </div>
     </div>
     <div class="todo-item">
      <div class="checkbox" onclick="toggleTodo(this)"></div>
      <div class="todo-content">
       <div class="todo-main-row">
        <div class="todo-text">
         Vacuum living room
        </div>
       </div>
      </div>
     </div>
    </div>
    <div class="section">
     <div class="section-header">
      Weekly Meal Planner
     </div>
     <div class="meal-grid">
      <div class="meal-day">
       <div class="meal-day-name">
        Mon
       </div>
       <div>
        Pasta
       </div>
      </div>
      <div class="meal-day">
       <div class="meal-day-name">
        Tue
       </div>
       <div>
        Salad
       </div>
      </div>
      <div class="meal-day">
       <div class="meal-day-name">
        Wed
       </div>
       <div>
        Stir-fry
       </div>
      </div>
      <div class="meal-day">
       <div class="meal-day-name">
        Thu
       </div>
       <div>
        Soup
       </div>
      </div>
      <div class="meal-day">
       <div class="meal-day-name">
        Fri
       </div>
       <div>
        Pizza
       </div>
      </div>
      <div class="meal-day">
       <div class="meal-day-name">
        Sat
       </div>
       <div>
        BBQ
       </div>
      </div>
      <div class="meal-day">
       <div class="meal-day-name">
        Sun
       </div>
       <div>
        Roast
       </div>
      </div>
     </div>
    </div>
    <div class="section">
     <div class="section-header">
      Grocery List
     </div>
     <div class="todo-item">
      <div class="checkbox" onclick="toggleTodo(this)"></div>
      <div class="todo-content">
       <div class="todo-main-row">
        <div class="todo-text">
         Milk
        </div>
       </div>
      </div>
     </div>
     <div class="todo-item">
      <div class="checkbox checked" onclick="toggleTodo(this)"></div>
      <div class="todo-content">
       <div class="todo-main-row">
        <div class="todo-text completed">
         Bread
        </div>
       </div>
      </div>
     </div><button class="add-btn" onclick="showAddGroceryForm()">+ Add Item</button>
    </div>
   </div><!-- Tab Navigation -->
   <div class="tab-navigation"><!-- Home: Heart symbol representing the center of your wellness journey --> <button class="tab-btn active" onclick="switchTab('home')">
     <svg width="32" height="32" viewbox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round" />
     </svg></button> <!-- Planner: Calendar grid representing organization, structure, and planning --> <button class="tab-btn" onclick="switchTab('planner')">
     <svg width="32" height="32" viewbox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><rect x="3" y="4" width="18" height="18" rx="2" ry="2" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round" /> <line x1="16" y1="2" x2="16" y2="6" stroke-width="1.2" stroke-linecap="round" /> <line x1="8" y1="2" x2="8" y2="6" stroke-width="1.2" stroke-linecap="round" /> <line x1="3" y1="10" x2="21" y2="10" stroke-width="1.2" stroke-linecap="round" /> <line x1="8" y1="14" x2="8" y2="14" stroke-width="1.2" stroke-linecap="round" /> <line x1="12" y1="14" x2="12" y2="14" stroke-width="1.2" stroke-linecap="round" /> <line x1="16" y1="14" x2="16" y2="14" stroke-width="1.2" stroke-linecap="round" /> <line x1="8" y1="18" x2="8" y2="18" stroke-width="1.2" stroke-linecap="round" /> <line x1="12" y1="18" x2="12" y2="18" stroke-width="1.2" stroke-linecap="round" />
     </svg></button> <!-- Goals: Mountain range with multiple peaks representing aspirations and achievements --> <button class="tab-btn" onclick="switchTab('goals')">
     <svg width="32" height="32" viewbox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M2 20h20" stroke-width="1.2" stroke-linecap="round" /> <path d="M2 20l4-8 3 4 4-10 3 6 4-8 4 6" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round" /> <circle cx="13" cy="6" r="1" stroke-width="1.2" />
     </svg></button> <!-- Habits: Infinity symbol with center dot representing continuous growth and balance --> <button class="tab-btn" onclick="switchTab('habits')">
     <svg width="32" height="32" viewbox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><circle cx="12" cy="12" r="3" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round" /> <path d="M12 1v6m0 6v6" stroke-width="1.2" stroke-linecap="round" /> <path d="M21 12h-6m-6 0H3" stroke-width="1.2" stroke-linecap="round" /> <path d="M18.36 5.64l-4.24 4.24m-4.24 4.24L5.64 18.36" stroke-width="1.2" stroke-linecap="round" /> <path d="M18.36 18.36l-4.24-4.24m-4.24-4.24L5.64 5.64" stroke-width="1.2" stroke-linecap="round" />
     </svg></button> <!-- Finances: Dollar sign representing money and financial management --> <button class="tab-btn" onclick="switchTab('finances')">
     <svg width="32" height="32" viewbox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><line x1="12" y1="1" x2="12" y2="23" stroke-width="1.2" stroke-linecap="round" /> <path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round" />
     </svg></button> <!-- Lifestyle: Flowing wave representing harmony, balance, and life's natural rhythm --> <button class="tab-btn" onclick="switchTab('lifestyle')">
     <svg width="32" height="32" viewbox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M2 12c2-4 6-4 8 0s6 4 8 0 6-4 8 0" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round" /> <path d="M2 16c2-2 4-2 6 0s4 2 6 0 4-2 6 0 4 2 6 0" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round" /> <path d="M2 8c1.5-2 3.5-2 5 0s3.5 2 5 0 3.5-2 5 0 3.5 2 5 0" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round" />
     </svg></button>
   </div>
  </div>
  <script>
        // Global Variables
        let todoIdCounter = 1;
        let scheduleIdCounter = 1;
        let planIdCounter = 1;
        let editingTodo = null;
        let editingSchedule = null;
        let editingTimeBlock = null;
        let currentDate = new Date();

        // Utility Functions
        function getDateKey(date) {
            return `${date.getFullYear()}-${date.getMonth()}-${date.getDate()}`;
        }

        function formatTo12Hour(time24) {
            const [hours, minutes] = time24.split(':');
            const hour = parseInt(hours);
            const ampm = hour >= 12 ? 'PM' : 'AM';
            const hour12 = hour % 12 || 12;
            return `${hour12}:${minutes} ${ampm}`;
        }

        function convertTo24Hour(time12) {
            const [time, ampm] = time12.split(' ');
            const [hours, minutes] = time.split(':');
            let hour = parseInt(hours);
            
            if (ampm === 'PM' && hour !== 12) {
                hour += 12;
            } else if (ampm === 'AM' && hour === 12) {
                hour = 0;
            }
            
            return `${hour.toString().padStart(2, '0')}:${minutes}`;
        }

        // Storage Functions
        function saveTasksForDate(date, tasks) {
            const dateKey = getDateKey(date);
            localStorage.setItem(`tasks_${dateKey}`, JSON.stringify(tasks));
        }

        function getTasksForDate(date) {
            const dateKey = getDateKey(date);
            const saved = localStorage.getItem(`tasks_${dateKey}`);
            return saved ? JSON.parse(saved) : [];
        }

        function saveScheduleForDate(date, schedule) {
            const dateKey = getDateKey(date);
            localStorage.setItem(`schedule_${dateKey}`, JSON.stringify(schedule));
        }

        function getScheduleForDate(date) {
            const dateKey = getDateKey(date);
            const saved = localStorage.getItem(`schedule_${dateKey}`);
            return saved ? JSON.parse(saved) : [];
        }

        function saveTimeBlocksForDate(date, timeBlocks) {
            const dateKey = getDateKey(date);
            localStorage.setItem(`timeblocks_${dateKey}`, JSON.stringify(timeBlocks));
        }

        function getTimeBlocksForDate(date) {
            const dateKey = getDateKey(date);
            const saved = localStorage.getItem(`timeblocks_${dateKey}`);
            return saved ? JSON.parse(saved) : { morning: [], afternoon: [], evening: [] };
        }

        // Calendar Functions
        function generateCalendar() {
            const calendarStrip = document.getElementById('calendarStrip');
            const today = new Date();
            const days = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
            
            calendarStrip.innerHTML = '';
            
            for (let i = -3; i <= 10; i++) {
                const date = new Date(today);
                date.setDate(today.getDate() + i);
                
                const dayElement = document.createElement('div');
                dayElement.className = `calendar-day ${i === 0 ? 'active' : ''}`;
                dayElement.onclick = () => selectDate(dayElement, date);
                
                dayElement.innerHTML = `
                    <div class="day-name">${days[date.getDay()]}</div>
                    <div class="day-number">${date.getDate()}</div>
                `;
                
                calendarStrip.appendChild(dayElement);
            }
        }

        function selectDate(element, date) {
            document.querySelectorAll('.calendar-day').forEach(day => {
                day.classList.remove('active');
            });
            element.classList.add('active');
            
            currentDate = date;
            loadTasksForDate(date);
            updateWellnessDisplay();
        }

        // Task Management Functions
        function loadTasksForDate(date) {
            const tasks = getTasksForDate(date);
            const todoList = document.getElementById('todoList');
            
            todoList.innerHTML = '';
            
            tasks.forEach(task => {
                const newTodo = document.createElement('div');
                newTodo.className = 'todo-item';
                newTodo.setAttribute('data-id', task.id);
                
                const priorityIndicator = task.priority ? '<div class="priority-indicator"></div>' : '';
                
                newTodo.innerHTML = `
                    <div class="checkbox ${task.completed ? 'checked' : ''}" onclick="toggleTodo(this)"></div>
                    <div class="todo-content">
                        <div class="todo-main-row">
                            <div class="todo-text ${task.completed ? 'completed' : ''}" onclick="editTodo(this)">${task.text}</div>
                            ${priorityIndicator}
                        </div>
                        ${task.category ? `<div class="todo-tag">${task.category}</div>` : ''}
                    </div>
                    <button class="delete-btn" onclick="deleteTodo(this.parentElement)">×</button>
                `;
                
                todoList.appendChild(newTodo);
            });
            
            loadScheduleForDate(date);
            loadTimeBlocksForDate(date);
            updateProgress();
        }

        function saveCurrentTasks() {
            const tasks = [];
            const todoItems = document.querySelectorAll('#todoList .todo-item');
            
            todoItems.forEach(item => {
                const checkbox = item.querySelector('.checkbox');
                const text = item.querySelector('.todo-text').textContent;
                const categoryElement = item.querySelector('.todo-tag');
                const category = categoryElement ? categoryElement.textContent : '';
                const id = item.getAttribute('data-id');
                const hasPriority = item.querySelector('.priority-indicator') !== null;
                
                tasks.push({
                    id: id,
                    text: text,
                    category: category,
                    completed: checkbox.classList.contains('checked'),
                    priority: hasPriority
                });
            });
            
            saveTasksForDate(currentDate, tasks);
        }

        function showAddTodoForm() {
            document.getElementById('addTodoForm').style.display = 'block';
            document.getElementById('taskInput').focus();
        }

        function cancelAddTodo() {
            document.getElementById('addTodoForm').style.display = 'none';
            document.getElementById('taskInput').value = '';
            document.getElementById('tagSelect').value = '';
            document.getElementById('priorityCheckbox').checked = false;
        }

        function saveTodo() {
            const taskInput = document.getElementById('taskInput');
            const tagSelect = document.getElementById('tagSelect');
            const priorityCheckbox = document.getElementById('priorityCheckbox');
            
            if (taskInput.value.trim()) {
                const todoList = document.getElementById('todoList');
                const newTodo = document.createElement('div');
                newTodo.className = 'todo-item';
                newTodo.setAttribute('data-id', todoIdCounter++);
                
                const priorityIndicator = priorityCheckbox.checked ? '<div class="priority-indicator"></div>' : '';
                
                newTodo.innerHTML = `
                    <div class="checkbox" onclick="toggleTodo(this)"></div>
                    <div class="todo-content">
                        <div class="todo-main-row">
                            <div class="todo-text" onclick="editTodo(this)">${taskInput.value.trim()}</div>
                            ${priorityIndicator}
                        </div>
                        ${tagSelect.value ? `<div class="todo-tag">${tagSelect.options[tagSelect.selectedIndex].text.toLowerCase()}</div>` : ''}
                    </div>
                    <button class="delete-btn" onclick="deleteTodo(this.parentElement)">×</button>
                `;
                
                todoList.appendChild(newTodo);
                cancelAddTodo();
                updateProgress();
                saveCurrentTasks();
            }
        }

        function toggleTodo(checkbox) {
            checkbox.classList.toggle('checked');
            const todoText = checkbox.parentElement.querySelector('.todo-text');
            todoText.classList.toggle('completed');
            updateProgress();
            saveCurrentTasks();
        }

        function editTodo(todoTextElement) {
            if (editingTodo) return;
            
            editingTodo = todoTextElement;
            const originalText = todoTextElement.textContent;
            
            const input = document.createElement('input');
            input.type = 'text';
            input.className = 'edit-input';
            input.value = originalText;
            
            todoTextElement.style.display = 'none';
            todoTextElement.parentElement.insertBefore(input, todoTextElement);
            input.focus();
            input.select();
            
            function saveEdit() {
                if (input.value.trim()) {
                    todoTextElement.textContent = input.value.trim();
                }
                todoTextElement.style.display = 'block';
                input.remove();
                editingTodo = null;
                saveCurrentTasks();
            }
            
            function cancelEdit() {
                todoTextElement.style.display = 'block';
                input.remove();
                editingTodo = null;
            }
            
            input.addEventListener('blur', saveEdit);
            input.addEventListener('keydown', function(e) {
                if (e.key === 'Enter') {
                    saveEdit();
                } else if (e.key === 'Escape') {
                    cancelEdit();
                }
            });
        }

        function deleteTodo(todoItem) {
            todoItem.remove();
            updateProgress();
            saveCurrentTasks();
        }

        function updateProgress() {
            const todoItems = document.querySelectorAll('#todoList .checkbox');
            const completedItems = document.querySelectorAll('#todoList .checkbox.checked');
            if (todoItems.length > 0) {
                const progress = (completedItems.length / todoItems.length) * 100;
                document.getElementById('todoProgress').style.width = progress + '%';
            } else {
                document.getElementById('todoProgress').style.width = '0%';
            }
        }

        // Schedule Management Functions
        function loadScheduleForDate(date) {
            const schedule = getScheduleForDate(date);
            const scheduleList = document.getElementById('scheduleList');
            
            scheduleList.innerHTML = '';
            
            schedule.sort((a, b) => a.time.localeCompare(b.time));
            
            schedule.forEach(item => {
                const scheduleItem = document.createElement('div');
                scheduleItem.className = 'schedule-item';
                scheduleItem.setAttribute('data-id', item.id);
                
                const timeDisplay = formatTo12Hour(item.time.substring(0, 5));
                
                scheduleItem.innerHTML = `
                    <div class="schedule-time" onclick="editScheduleTime(this)" data-24hour="${item.time.substring(0, 5)}">${timeDisplay}</div>
                    <div class="schedule-event" onclick="editScheduleEvent(this)">${item.event}</div>
                    <button class="delete-btn" onclick="deleteScheduleItem(this.parentElement)">×</button>
                `;
                
                scheduleList.appendChild(scheduleItem);
            });
        }

        function saveCurrentSchedule() {
            const schedule = [];
            const scheduleItems = document.querySelectorAll('#scheduleList .schedule-item');
            
            scheduleItems.forEach(item => {
                const timeElement = item.querySelector('.schedule-time');
                const time24 = timeElement.getAttribute('data-24hour') || convertTo24Hour(timeElement.textContent);
                const event = item.querySelector('.schedule-event').textContent;
                const id = item.getAttribute('data-id');
                
                schedule.push({
                    id: id,
                    time: time24 + ':00',
                    event: event
                });
            });
            
            saveScheduleForDate(currentDate, schedule);
        }

        function showAddScheduleForm() {
            document.getElementById('addScheduleForm').style.display = 'block';
            document.getElementById('timeInput').focus();
        }

        function cancelAddSchedule() {
            document.getElementById('addScheduleForm').style.display = 'none';
            document.getElementById('timeInput').value = '';
            document.getElementById('eventInput').value = '';
        }

        function saveSchedule() {
            const timeInput = document.getElementById('timeInput');
            const eventInput = document.getElementById('eventInput');
            
            if (timeInput.value && eventInput.value.trim()) {
                const scheduleList = document.getElementById('scheduleList');
                const newScheduleItem = document.createElement('div');
                newScheduleItem.className = 'schedule-item';
                newScheduleItem.setAttribute('data-id', scheduleIdCounter++);
                
                const timeDisplay = formatTo12Hour(timeInput.value + ':00');
                
                newScheduleItem.innerHTML = `
                    <div class="schedule-time" onclick="editScheduleTime(this)" data-24hour="${timeInput.value}">${timeDisplay}</div>
                    <div class="schedule-event" onclick="editScheduleEvent(this)">${eventInput.value.trim()}</div>
                    <button class="delete-btn" onclick="deleteScheduleItem(this.parentElement)">×</button>
                `;
                
                scheduleList.appendChild(newScheduleItem);
                cancelAddSchedule();
                
                saveCurrentSchedule();
                loadScheduleForDate(currentDate);
            }
        }

        function editScheduleTime(timeElement) {
            if (editingSchedule) return;
            
            editingSchedule = timeElement;
            const current24Hour = timeElement.getAttribute('data-24hour');
            
            const input = document.createElement('input');
            input.type = 'time';
            input.className = 'edit-input';
            input.value = current24Hour;
            input.style.width = '80px';
            
            timeElement.style.display = 'none';
            timeElement.parentElement.insertBefore(input, timeElement);
            input.focus();
            
            function saveEdit() {
                if (input.value) {
                    const new12Hour = formatTo12Hour(input.value + ':00');
                    timeElement.textContent = new12Hour;
                    timeElement.setAttribute('data-24hour', input.value);
                    saveCurrentSchedule();
                    loadScheduleForDate(currentDate);
                }
                timeElement.style.display = 'block';
                input.remove();
                editingSchedule = null;
            }
            
            function cancelEdit() {
                timeElement.style.display = 'block';
                input.remove();
                editingSchedule = null;
            }
            
            input.addEventListener('blur', saveEdit);
            input.addEventListener('keydown', function(e) {
                if (e.key === 'Enter') {
                    saveEdit();
                } else if (e.key === 'Escape') {
                    cancelEdit();
                }
            });
        }

        function editScheduleEvent(eventElement) {
            if (editingSchedule) return;
            
            editingSchedule = eventElement;
            const originalEvent = eventElement.textContent;
            
            const input = document.createElement('input');
            input.type = 'text';
            input.className = 'edit-input';
            input.value = originalEvent;
            
            eventElement.style.display = 'none';
            eventElement.parentElement.insertBefore(input, eventElement);
            input.focus();
            input.select();
            
            function saveEdit() {
                if (input.value.trim()) {
                    eventElement.textContent = input.value.trim();
                }
                eventElement.style.display = 'block';
                input.remove();
                editingSchedule = null;
                saveCurrentSchedule();
            }
            
            function cancelEdit() {
                eventElement.style.display = 'block';
                input.remove();
                editingSchedule = null;
            }
            
            input.addEventListener('blur', saveEdit);
            input.addEventListener('keydown', function(e) {
                if (e.key === 'Enter') {
                    saveEdit();
                } else if (e.key === 'Escape') {
                    cancelEdit();
                }
            });
        }

        function deleteScheduleItem(scheduleItem) {
            scheduleItem.remove();
            saveCurrentSchedule();
        }

        // Time Block Management Functions - Fixed and Working
        function loadTimeBlocksForDate(date) {
            const timeBlocks = getTimeBlocksForDate(date);
            
            // Load morning plans
            loadPlansForPeriod('morning', timeBlocks.morning || []);
            // Load afternoon plans  
            loadPlansForPeriod('afternoon', timeBlocks.afternoon || []);
            // Load evening plans
            loadPlansForPeriod('evening', timeBlocks.evening || []);
        }

        function loadPlansForPeriod(period, plans) {
            const container = document.getElementById(period + '-plans');
            if (!container) return;
            
            container.innerHTML = '';
            plans.forEach(plan => {
                createPlanElement(container, plan);
            });
        }

        function createPlanElement(container, plan) {
            const planDiv = document.createElement('div');
            planDiv.className = 'time-block-plan';
            planDiv.setAttribute('data-id', plan.id);
            
            planDiv.innerHTML = `
                <div class="plan-checkbox ${plan.completed ? 'checked' : ''}" onclick="togglePlan('${plan.id}')"></div>
                <div class="plan-text ${plan.completed ? 'completed' : ''}" onclick="editPlan('${plan.id}')">${plan.text}</div>
                <button class="plan-delete" onclick="deletePlan('${plan.id}')">×</button>
            `;
            
            container.appendChild(planDiv);
        }

        function saveTimeBlocks() {
            const timeBlocks = { morning: [], afternoon: [], evening: [] };
            
            ['morning', 'afternoon', 'evening'].forEach(period => {
                const plans = document.querySelectorAll(`#${period}-plans .time-block-plan`);
                plans.forEach(planElement => {
                    const checkbox = planElement.querySelector('.plan-checkbox');
                    const text = planElement.querySelector('.plan-text');
                    const id = planElement.getAttribute('data-id');
                    
                    timeBlocks[period].push({
                        id: id,
                        text: text.textContent,
                        completed: checkbox.classList.contains('checked')
                    });
                });
            });
            
            saveTimeBlocksForDate(currentDate, timeBlocks);
        }

        // Individual functions for each time period
        function addMorningPlan() {
            showCustomPrompt('Enter your morning plan:', function(text) {
                if (text && text.trim()) {
                    addPlanToPeriod('morning', text.trim());
                }
            });
        }

        function addAfternoonPlan() {
            showCustomPrompt('Enter your afternoon plan:', function(text) {
                if (text && text.trim()) {
                    addPlanToPeriod('afternoon', text.trim());
                }
            });
        }

        function addEveningPlan() {
            showCustomPrompt('Enter your evening plan:', function(text) {
                if (text && text.trim()) {
                    addPlanToPeriod('evening', text.trim());
                }
            });
        }

        function addPlanToPeriod(period, text) {
            const container = document.getElementById(period + '-plans');
            if (!container) return;
            
            const newPlan = {
                id: 'plan_' + Date.now() + '_' + Math.random().toString(36).substr(2, 5),
                text: text,
                completed: false
            };
            
            createPlanElement(container, newPlan);
            saveTimeBlocks();
        }

        function togglePlan(planId) {
            const planElement = document.querySelector(`[data-id="${planId}"]`);
            if (!planElement) return;
            
            const checkbox = planElement.querySelector('.plan-checkbox');
            const text = planElement.querySelector('.plan-text');
            
            checkbox.classList.toggle('checked');
            text.classList.toggle('completed');
            saveTimeBlocks();
        }

        function editPlan(planId) {
            const planElement = document.querySelector(`[data-id="${planId}"]`);
            if (!planElement) return;
            
            const textElement = planElement.querySelector('.plan-text');
            const currentText = textElement.textContent;
            
            showCustomPrompt(`Edit plan: (Current: ${currentText})`, function(newText) {
                if (newText && newText.trim()) {
                    textElement.textContent = newText.trim();
                    saveTimeBlocks();
                    showToast('Plan updated successfully!');
                }
            });
        }

        function deletePlan(planId) {
            const planElement = document.querySelector(`[data-id="${planId}"]`);
            if (planElement) {
                planElement.remove();
                saveTimeBlocks();
            }
        }

        // UI Functions
        function switchTab(tabName) {
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            
            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            document.getElementById(tabName).classList.add('active');
            
            // Find and activate the correct tab button
            const tabButtons = document.querySelectorAll('.tab-btn');
            tabButtons.forEach(btn => {
                const onclickAttr = btn.getAttribute('onclick');
                if (onclickAttr && onclickAttr.includes(`'${tabName}'`)) {
                    btn.classList.add('active');
                }
            });
            
            // Show/hide calendar strip based on tab
            const calendarStrip = document.getElementById('calendarStrip');
            const backArrow = document.getElementById('backArrow');
            
            if (tabName === 'home') {
                calendarStrip.style.display = 'flex';
                backArrow.classList.remove('show');
            } else {
                calendarStrip.style.display = 'none';
                backArrow.classList.add('show');
            }
        }

        function goBackToHome() {
            // Use the switchTab function to ensure consistent behavior
            switchTab('home');
        }

        function switchPlannerTab(tabName) {
            document.querySelectorAll('.planner-content').forEach(content => {
                content.classList.remove('active');
            });
            
            document.querySelectorAll('.planner-tab-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            document.getElementById(tabName + '-planner').classList.add('active');
            event.target.classList.add('active');
        }

        function toggleHabit(habitItem) {
            habitItem.classList.toggle('completed');
        }

        function toggleDay(dayCircle) {
            dayCircle.classList.toggle('completed');
        }

        // Theme System
        function changeTheme(themeName) {
            document.body.classList.remove('theme-blue', 'theme-green', 'theme-pink', 'theme-beige');
            document.body.classList.add('theme-' + themeName);
            
            document.querySelectorAll('.theme-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            document.querySelector('.theme-btn.' + themeName).classList.add('active');
            
            localStorage.setItem('selectedTheme', themeName);
        }

        function loadTheme() {
            const savedTheme = localStorage.getItem('selectedTheme') || 'pink';
            changeTheme(savedTheme);
        }

        // Daily Priority Management
        function loadDailyPriorities() {
            const priorities = JSON.parse(localStorage.getItem('dailyPriorities')) || [];
            const prioritiesList = document.getElementById('dailyPrioritiesList');
            
            prioritiesList.innerHTML = '';
            priorities.forEach(priority => {
                const priorityItem = document.createElement('div');
                priorityItem.className = 'todo-item';
                priorityItem.setAttribute('data-priority-id', priority.id);
                
                priorityItem.innerHTML = `
                    <div class="checkbox ${priority.completed ? 'checked' : ''}" onclick="toggleDailyPriority('${priority.id}')"></div>
                    <div class="todo-content">
                        <div class="todo-main-row">
                            <div class="todo-text ${priority.completed ? 'completed' : ''}" onclick="editDailyPriority('${priority.id}', this)">${priority.text}</div>
                        </div>
                    </div>
                    <button class="delete-btn" onclick="deleteDailyPriority('${priority.id}')">×</button>
                `;
                
                prioritiesList.appendChild(priorityItem);
            });
        }

        function saveDailyPriorities() {
            const priorities = [];
            const priorityItems = document.querySelectorAll('#dailyPrioritiesList .todo-item');
            
            priorityItems.forEach(item => {
                const checkbox = item.querySelector('.checkbox');
                const text = item.querySelector('.todo-text').textContent;
                const id = item.getAttribute('data-priority-id');
                
                priorities.push({
                    id: id,
                    text: text,
                    completed: checkbox.classList.contains('checked')
                });
            });
            
            localStorage.setItem('dailyPriorities', JSON.stringify(priorities));
        }

        function showAddDailyPriorityForm() {
            document.getElementById('addDailyPriorityForm').style.display = 'block';
            document.getElementById('dailyPriorityInput').focus();
        }

        function cancelAddDailyPriority() {
            document.getElementById('addDailyPriorityForm').style.display = 'none';
            document.getElementById('dailyPriorityInput').value = '';
        }

        function saveDailyPriority() {
            const priorityInput = document.getElementById('dailyPriorityInput');
            
            if (priorityInput.value.trim()) {
                const priorities = JSON.parse(localStorage.getItem('dailyPriorities')) || [];
                const newPriority = {
                    id: 'priority_' + Date.now(),
                    text: priorityInput.value.trim(),
                    completed: false
                };
                
                priorities.push(newPriority);
                localStorage.setItem('dailyPriorities', JSON.stringify(priorities));
                
                cancelAddDailyPriority();
                loadDailyPriorities();
                showToast('Priority added successfully!');
            }
        }

        function toggleDailyPriority(priorityId) {
            const priorities = JSON.parse(localStorage.getItem('dailyPriorities')) || [];
            const priority = priorities.find(p => p.id === priorityId);
            
            if (priority) {
                priority.completed = !priority.completed;
                localStorage.setItem('dailyPriorities', JSON.stringify(priorities));
                loadDailyPriorities();
            }
        }

        function editDailyPriority(priorityId, textElement) {
            if (editingTodo) return;
            
            editingTodo = textElement;
            const originalText = textElement.textContent;
            
            const input = document.createElement('input');
            input.type = 'text';
            input.className = 'edit-input';
            input.value = originalText;
            
            textElement.style.display = 'none';
            textElement.parentElement.insertBefore(input, textElement);
            input.focus();
            input.select();
            
            function saveEdit() {
                if (input.value.trim()) {
                    const priorities = JSON.parse(localStorage.getItem('dailyPriorities')) || [];
                    const priority = priorities.find(p => p.id === priorityId);
                    
                    if (priority) {
                        priority.text = input.value.trim();
                        localStorage.setItem('dailyPriorities', JSON.stringify(priorities));
                        textElement.textContent = input.value.trim();
                    }
                }
                textElement.style.display = 'block';
                input.remove();
                editingTodo = null;
            }
            
            function cancelEdit() {
                textElement.style.display = 'block';
                input.remove();
                editingTodo = null;
            }
            
            input.addEventListener('blur', saveEdit);
            input.addEventListener('keydown', function(e) {
                if (e.key === 'Enter') {
                    saveEdit();
                } else if (e.key === 'Escape') {
                    cancelEdit();
                }
            });
        }

        function deleteDailyPriority(priorityId) {
            const priorities = JSON.parse(localStorage.getItem('dailyPriorities')) || [];
            const updatedPriorities = priorities.filter(p => p.id !== priorityId);
            localStorage.setItem('dailyPriorities', JSON.stringify(updatedPriorities));
            loadDailyPriorities();
            showToast('Priority deleted successfully!');
        }

        // Modal and Toast Functions
        function showCustomPrompt(message, callback) {
            const modal = document.createElement('div');
            modal.className = 'custom-modal';
            
            const dialog = document.createElement('div');
            dialog.className = 'modal-dialog';
            
            dialog.innerHTML = `
                <div class="modal-message">${message}</div>
                <input type="text" class="modal-input" id="promptInput">
                <div class="modal-buttons">
                    <button class="modal-btn" onclick="closePrompt()">Cancel</button>
                    <button class="modal-btn primary" onclick="submitPrompt()">OK</button>
                </div>
            `;
            
            modal.appendChild(dialog);
            document.body.appendChild(modal);
            
            const input = dialog.querySelector('#promptInput');
            input.focus();
            
            window.currentPromptCallback = callback;
            window.currentPromptModal = modal;
            
            input.addEventListener('keydown', (e) => {
                if (e.key === 'Enter') submitPrompt();
                if (e.key === 'Escape') closePrompt();
            });
        }

        function submitPrompt() {
            const input = document.querySelector('#promptInput');
            const value = input ? input.value.trim() : '';
            const callback = window.currentPromptCallback;
            closePrompt();
            if (callback) {
                callback(value);
            }
        }

        function closePrompt() {
            if (window.currentPromptModal) {
                document.body.removeChild(window.currentPromptModal);
                window.currentPromptModal = null;
                window.currentPromptCallback = null;
            }
        }

        function showToast(message) {
            const toast = document.createElement('div');
            toast.className = 'toast';
            toast.textContent = message;
            document.body.appendChild(toast);
            setTimeout(() => {
                if (document.body.contains(toast)) {
                    document.body.removeChild(toast);
                }
            }, 2000);
        }

        // Additional Functions
        function showAddExpenseForm() {
            const item = prompt('Enter expense item:');
            if (item && item.trim()) {
                const amount = prompt('Enter amount (e.g., 25.50):');
                if (amount && amount.trim()) {
                    // Add expense to the table
                    const expenseTable = document.querySelector('.expense-table');
                    const today = new Date();
                    const dateStr = `${today.getMonth() + 1}/${today.getDate()}`;
                    
                    const newRow = document.createElement('div');
                    newRow.className = 'expense-row';
                    newRow.innerHTML = `
                        <div>${dateStr}</div>
                        <div>${item.trim()}</div>
                        <div>Other</div>
                        <div>$${parseFloat(amount).toFixed(2)}</div>
                    `;
                    
                    expenseTable.appendChild(newRow);
                    showToast('Expense added successfully!');
                }
            }
        }

        function showAddGroceryForm() {
            const item = prompt('Enter grocery item:');
            if (item && item.trim()) {
                const grocerySection = document.querySelector('#lifestyle .section:last-child');
                const newItem = document.createElement('div');
                newItem.className = 'todo-item';
                newItem.innerHTML = `
                    <div class="checkbox" onclick="toggleTodo(this)"></div>
                    <div class="todo-content">
                        <div class="todo-main-row">
                            <div class="todo-text">${item.trim()}</div>
                        </div>
                    </div>
                `;
                grocerySection.insertBefore(newItem, grocerySection.lastElementChild);
                showToast('Item added to grocery list!');
            }
        }

        // Habit Management Functions
        function loadHabits() {
            const habits = JSON.parse(localStorage.getItem('habits')) || [];
            
            const habitTable = document.getElementById('habitTrackerTable');
            habitTable.innerHTML = '';
            
            habits.forEach(habit => {
                const habitRow = document.createElement('div');
                habitRow.className = 'habit-row';
                habitRow.setAttribute('data-habit-id', habit.id);
                
                const habitData = getHabitDataForWeek(habit.id);
                const days = ['M', 'T', 'W', 'T', 'F', 'S', 'S'];
                
                let dayCircles = '';
                days.forEach((day, index) => {
                    const isCompleted = habitData[index] ? 'completed' : '';
                    dayCircles += `<div class="day-circle ${isCompleted}" onclick="toggleHabitDay('${habit.id}', ${index})">${day}</div>`;
                });
                
                habitRow.innerHTML = `
                    <div class="habit-label">${habit.name}</div>
                    <div class="habit-days">
                        ${dayCircles}
                    </div>
                    <button class="habit-delete" onclick="deleteHabit('${habit.id}')">×</button>
                `;
                
                habitTable.appendChild(habitRow);
            });
        }

        function getHabitDataForWeek(habitId) {
            const today = new Date();
            const startOfWeek = new Date(today);
            startOfWeek.setDate(today.getDate() - today.getDay() + 1); // Monday
            
            const weekData = [];
            for (let i = 0; i < 7; i++) {
                const date = new Date(startOfWeek);
                date.setDate(startOfWeek.getDate() + i);
                const dateKey = getDateKey(date);
                const habitData = JSON.parse(localStorage.getItem(`habit_${habitId}_${dateKey}`)) || false;
                weekData.push(habitData);
            }
            return weekData;
        }

        function toggleHabitDay(habitId, dayIndex) {
            const today = new Date();
            const startOfWeek = new Date(today);
            startOfWeek.setDate(today.getDate() - today.getDay() + 1); // Monday
            
            const targetDate = new Date(startOfWeek);
            targetDate.setDate(startOfWeek.getDate() + dayIndex);
            const dateKey = getDateKey(targetDate);
            
            const currentState = JSON.parse(localStorage.getItem(`habit_${habitId}_${dateKey}`)) || false;
            localStorage.setItem(`habit_${habitId}_${dateKey}`, JSON.stringify(!currentState));
            
            loadHabits();
        }

        function showAddHabitForm() {
            document.getElementById('addHabitForm').style.display = 'block';
            document.getElementById('habitNameInput').focus();
        }

        function cancelAddHabit() {
            document.getElementById('addHabitForm').style.display = 'none';
            document.getElementById('habitNameInput').value = '';
        }

        function saveHabit() {
            const habitNameInput = document.getElementById('habitNameInput');
            
            if (habitNameInput.value.trim()) {
                const habits = JSON.parse(localStorage.getItem('habits')) || [];
                const newHabit = {
                    id: 'habit_' + Date.now(),
                    name: habitNameInput.value.trim()
                };
                
                habits.push(newHabit);
                localStorage.setItem('habits', JSON.stringify(habits));
                
                cancelAddHabit();
                loadHabits();
                showToast('Habit added successfully!');
            }
        }

        function deleteHabit(habitId) {
            const habits = JSON.parse(localStorage.getItem('habits')) || [];
            const updatedHabits = habits.filter(habit => habit.id !== habitId);
            localStorage.setItem('habits', JSON.stringify(updatedHabits));
            
            // Clean up habit data for all dates
            for (let i = 0; i < localStorage.length; i++) {
                const key = localStorage.key(i);
                if (key && key.startsWith(`habit_${habitId}_`)) {
                    localStorage.removeItem(key);
                    i--; // Adjust index since we removed an item
                }
            }
            
            loadHabits();
            showToast('Habit deleted successfully!');
        }

        // Routine Management Functions
        function loadRoutines() {
            loadMorningRoutine();
            loadNightRoutine();
            loadRoutineTimes();
        }

        function loadMorningRoutine() {
            const morningRoutine = JSON.parse(localStorage.getItem('morningRoutine')) || [];
            const morningList = document.getElementById('morningRoutineList');
            
            morningList.innerHTML = '';
            morningRoutine.forEach(item => {
                const routineItem = document.createElement('div');
                routineItem.className = 'todo-item';
                routineItem.setAttribute('data-id', item.id);
                
                routineItem.innerHTML = `
                    <div class="todo-content" style="margin-left: 0;">
                        <div class="todo-main-row">
                            <div class="todo-text" onclick="editRoutineItemInline('morning', '${item.id}', this)">${item.text}</div>
                        </div>
                    </div>
                    <button class="delete-btn" onclick="deleteRoutineItem('morning', '${item.id}')">×</button>
                `;
                
                morningList.appendChild(routineItem);
            });
        }

        function loadNightRoutine() {
            const nightRoutine = JSON.parse(localStorage.getItem('nightRoutine')) || [];
            const nightList = document.getElementById('nightRoutineList');
            
            nightList.innerHTML = '';
            nightRoutine.forEach(item => {
                const routineItem = document.createElement('div');
                routineItem.className = 'todo-item';
                routineItem.setAttribute('data-id', item.id);
                
                routineItem.innerHTML = `
                    <div class="todo-content" style="margin-left: 0;">
                        <div class="todo-main-row">
                            <div class="todo-text" onclick="editRoutineItemInline('night', '${item.id}', this)">${item.text}</div>
                        </div>
                    </div>
                    <button class="delete-btn" onclick="deleteRoutineItem('night', '${item.id}')">×</button>
                `;
                
                nightList.appendChild(routineItem);
            });
        }

        function loadRoutineTimes() {
            const morningTime = localStorage.getItem('morningRoutineTime') || '7:00 AM';
            const nightTime = localStorage.getItem('nightRoutineTime') || '10:00 PM';
            
            document.getElementById('morningTime').textContent = morningTime;
            document.getElementById('nightTime').textContent = nightTime;
        }

        function editRoutineTime(type) {
            const currentTime = localStorage.getItem(type + 'RoutineTime') || (type === 'morning' ? '7:00 AM' : '10:00 PM');
            
            showCustomPrompt(`Set ${type} routine time (e.g., 7:00 AM): (Current: ${currentTime})`, function(newTime) {
                if (newTime && newTime.trim()) {
                    localStorage.setItem(type + 'RoutineTime', newTime.trim());
                    loadRoutineTimes();
                    showToast(`${type.charAt(0).toUpperCase() + type.slice(1)} routine time updated!`);
                }
            });
        }

        function showAddMorningRoutineForm() {
            document.getElementById('addMorningRoutineForm').style.display = 'block';
            document.getElementById('morningRoutineInput').focus();
        }

        function cancelAddMorningRoutine() {
            document.getElementById('addMorningRoutineForm').style.display = 'none';
            document.getElementById('morningRoutineInput').value = '';
        }

        function saveMorningRoutine() {
            const input = document.getElementById('morningRoutineInput');
            if (input.value.trim()) {
                const morningRoutine = JSON.parse(localStorage.getItem('morningRoutine')) || [];
                const newItem = {
                    id: 'morning_' + Date.now(),
                    text: input.value.trim(),
                    completed: false
                };
                
                morningRoutine.push(newItem);
                localStorage.setItem('morningRoutine', JSON.stringify(morningRoutine));
                
                cancelAddMorningRoutine();
                loadMorningRoutine();
            }
        }

        function showAddNightRoutineForm() {
            document.getElementById('addNightRoutineForm').style.display = 'block';
            document.getElementById('nightRoutineInput').focus();
        }

        function cancelAddNightRoutine() {
            document.getElementById('addNightRoutineForm').style.display = 'none';
            document.getElementById('nightRoutineInput').value = '';
        }

        function saveNightRoutine() {
            const input = document.getElementById('nightRoutineInput');
            if (input.value.trim()) {
                const nightRoutine = JSON.parse(localStorage.getItem('nightRoutine')) || [];
                const newItem = {
                    id: 'night_' + Date.now(),
                    text: input.value.trim(),
                    completed: false
                };
                
                nightRoutine.push(newItem);
                localStorage.setItem('nightRoutine', JSON.stringify(nightRoutine));
                
                cancelAddNightRoutine();
                loadNightRoutine();
            }
        }



        function editRoutineItemInline(type, itemId, textElement) {
            if (editingTodo) return;
            
            editingTodo = textElement;
            const originalText = textElement.textContent;
            
            const input = document.createElement('input');
            input.type = 'text';
            input.className = 'edit-input';
            input.value = originalText;
            
            textElement.style.display = 'none';
            textElement.parentElement.insertBefore(input, textElement);
            input.focus();
            input.select();
            
            function saveEdit() {
                if (input.value.trim()) {
                    const routine = JSON.parse(localStorage.getItem(type + 'Routine')) || [];
                    const item = routine.find(r => r.id === itemId);
                    
                    if (item) {
                        item.text = input.value.trim();
                        localStorage.setItem(type + 'Routine', JSON.stringify(routine));
                        textElement.textContent = input.value.trim();
                    }
                }
                textElement.style.display = 'block';
                input.remove();
                editingTodo = null;
            }
            
            function cancelEdit() {
                textElement.style.display = 'block';
                input.remove();
                editingTodo = null;
            }
            
            input.addEventListener('blur', saveEdit);
            input.addEventListener('keydown', function(e) {
                if (e.key === 'Enter') {
                    saveEdit();
                } else if (e.key === 'Escape') {
                    cancelEdit();
                }
            });
        }

        function deleteRoutineItem(type, itemId) {
            const routine = JSON.parse(localStorage.getItem(type + 'Routine')) || [];
            const updatedRoutine = routine.filter(r => r.id !== itemId);
            localStorage.setItem(type + 'Routine', JSON.stringify(updatedRoutine));
            
            if (type === 'morning') {
                loadMorningRoutine();
            } else {
                loadNightRoutine();
            }
        }



        // Circular Calendar Variables
        let currentCalendarMonth = new Date().getMonth();
        let currentCalendarYear = new Date().getFullYear();
        let selectedCalendarDate = null;

        // Weekly Calendar Variables
        let currentWeekStart = new Date();
        let weeklyTaskIdCounter = 1;

        // Circular Calendar Functions
        function generateCircularCalendar() {
            const calendar = document.getElementById('circularCalendar');
            const monthYear = document.getElementById('calendarMonthYear');
            
            const monthNames = ['January', 'February', 'March', 'April', 'May', 'June',
                'July', 'August', 'September', 'October', 'November', 'December'];
            
            monthYear.textContent = `${monthNames[currentCalendarMonth]} ${currentCalendarYear}`;
            
            calendar.innerHTML = '';
            
            // Add month display inside circle
            const monthDisplay = document.createElement('div');
            monthDisplay.className = 'calendar-month-display';
            monthDisplay.textContent = `${monthNames[currentCalendarMonth]} ${currentCalendarYear}`;
            calendar.appendChild(monthDisplay);
            
            // Create calendar grid
            const calendarGrid = document.createElement('div');
            calendarGrid.className = 'calendar-grid';
            
            // Add day headers
            const dayHeaders = ['S', 'M', 'T', 'W', 'T', 'F', 'S'];
            dayHeaders.forEach(day => {
                const header = document.createElement('div');
                header.className = 'calendar-day-header';
                header.textContent = day;
                calendarGrid.appendChild(header);
            });
            
            // Get first day of month and days in month
            const firstDay = new Date(currentCalendarYear, currentCalendarMonth, 1).getDay();
            const daysInMonth = new Date(currentCalendarYear, currentCalendarMonth + 1, 0).getDate();
            const daysInPrevMonth = new Date(currentCalendarYear, currentCalendarMonth, 0).getDate();
            
            const today = new Date();
            
            // Add empty cells for days before the first day of the month
            for (let i = firstDay - 1; i >= 0; i--) {
                const dayElement = document.createElement('div');
                dayElement.className = 'calendar-day-circle other-month';
                dayElement.textContent = daysInPrevMonth - i;
                calendarGrid.appendChild(dayElement);
            }
            
            // Add current month's days
            for (let day = 1; day <= daysInMonth; day++) {
                const dayElement = document.createElement('div');
                dayElement.className = 'calendar-day-circle';
                dayElement.textContent = day;
                dayElement.onclick = () => openTaskPopup(day);
                
                // Check if this is today
                const dayDate = new Date(currentCalendarYear, currentCalendarMonth, day);
                const todayDate = new Date();
                todayDate.setHours(0, 0, 0, 0);
                dayDate.setHours(0, 0, 0, 0);
                
                if (dayDate.getTime() === todayDate.getTime()) {
                    dayElement.classList.add('today');
                }
                
                // Check if this day has tasks
                const dateKey = getCalendarDateKey(currentCalendarYear, currentCalendarMonth, day);
                const tasks = getCalendarTasks(dateKey);
                if (tasks.length > 0) {
                    dayElement.classList.add('has-tasks');
                }
                
                calendarGrid.appendChild(dayElement);
            }
            
            // Add days from next month to fill the grid
            const totalCells = 42; // 6 rows × 7 days
            const cellsUsed = firstDay + daysInMonth;
            const remainingCells = totalCells - cellsUsed;
            
            for (let day = 1; day <= remainingCells && calendarGrid.children.length < totalCells + 7; day++) {
                const dayElement = document.createElement('div');
                dayElement.className = 'calendar-day-circle other-month';
                dayElement.textContent = day;
                calendarGrid.appendChild(dayElement);
            }
            
            calendar.appendChild(calendarGrid);
        }

        function previousMonth() {
            currentCalendarMonth--;
            if (currentCalendarMonth < 0) {
                currentCalendarMonth = 11;
                currentCalendarYear--;
            }
            generateCircularCalendar();
        }

        function nextMonth() {
            currentCalendarMonth++;
            if (currentCalendarMonth > 11) {
                currentCalendarMonth = 0;
                currentCalendarYear++;
            }
            generateCircularCalendar();
        }

        function getCalendarDateKey(year, month, day) {
            return `calendar_${year}_${month}_${day}`;
        }

        function getCalendarTasks(dateKey) {
            const saved = localStorage.getItem(dateKey);
            return saved ? JSON.parse(saved) : [];
        }

        function saveCalendarTasks(dateKey, tasks) {
            localStorage.setItem(dateKey, JSON.stringify(tasks));
        }

        // Task Popup Functions
        function openTaskPopup(day) {
            selectedCalendarDate = { year: currentCalendarYear, month: currentCalendarMonth, day: day };
            const dateKey = getCalendarDateKey(currentCalendarYear, currentCalendarMonth, day);
            const tasks = getCalendarTasks(dateKey);
            
            const monthNames = ['January', 'February', 'March', 'April', 'May', 'June',
                'July', 'August', 'September', 'October', 'November', 'December'];
            
            const popup = document.createElement('div');
            popup.className = 'task-popup';
            popup.id = 'taskPopup';
            
            popup.innerHTML = `
                <div class="task-popup-content">
                    <div class="task-popup-header">
                        <div class="task-popup-title">${monthNames[currentCalendarMonth]} ${day}, ${currentCalendarYear}</div>
                        <button class="task-popup-close" onclick="closeTaskPopup()">×</button>
                    </div>
                    <div class="task-popup-list" id="taskPopupList">
                        <!-- Tasks will be loaded here -->
                    </div>
                    <div class="task-popup-add-form" id="taskPopupAddForm">
                        <input type="text" class="task-popup-input" id="taskPopupInput" placeholder="Enter task...">
                        <div class="task-popup-buttons">
                            <button class="task-popup-btn" onclick="cancelAddCalendarTask()">Cancel</button>
                            <button class="task-popup-btn primary" onclick="saveCalendarTask()">Add Task</button>
                        </div>
                    </div>
                    <button class="add-btn" onclick="showAddCalendarTaskForm()">+ Add Task</button>
                </div>
            `;
            
            document.body.appendChild(popup);
            loadTasksInPopup(tasks);
            
            // Close popup when clicking outside
            popup.addEventListener('click', (e) => {
                if (e.target === popup) {
                    closeTaskPopup();
                }
            });
        }

        function closeTaskPopup() {
            const popup = document.getElementById('taskPopup');
            if (popup) {
                document.body.removeChild(popup);
            }
            selectedCalendarDate = null;
        }

        function loadTasksInPopup(tasks) {
            const taskList = document.getElementById('taskPopupList');
            taskList.innerHTML = '';
            
            tasks.forEach((task, index) => {
                const taskItem = document.createElement('div');
                taskItem.className = 'task-popup-item';
                taskItem.setAttribute('data-index', index);
                
                taskItem.innerHTML = `
                    <div class="task-popup-checkbox ${task.completed ? 'checked' : ''}" onclick="toggleCalendarTask(${index})"></div>
                    <div class="task-popup-text ${task.completed ? 'completed' : ''}" onclick="editCalendarTask(${index}, this)">${task.text}</div>
                    <button class="task-popup-delete" onclick="deleteCalendarTask(${index})">×</button>
                `;
                
                taskList.appendChild(taskItem);
            });
        }

        function showAddCalendarTaskForm() {
            document.getElementById('taskPopupAddForm').style.display = 'block';
            document.getElementById('taskPopupInput').focus();
        }

        function cancelAddCalendarTask() {
            document.getElementById('taskPopupAddForm').style.display = 'none';
            document.getElementById('taskPopupInput').value = '';
        }

        function saveCalendarTask() {
            const input = document.getElementById('taskPopupInput');
            if (!input.value.trim() || !selectedCalendarDate) return;
            
            const dateKey = getCalendarDateKey(selectedCalendarDate.year, selectedCalendarDate.month, selectedCalendarDate.day);
            const tasks = getCalendarTasks(dateKey);
            
            tasks.push({
                id: Date.now(),
                text: input.value.trim(),
                completed: false
            });
            
            saveCalendarTasks(dateKey, tasks);
            loadTasksInPopup(tasks);
            cancelAddCalendarTask();
            generateCircularCalendar(); // Refresh to show task dot
        }

        function toggleCalendarTask(index) {
            if (!selectedCalendarDate) return;
            
            const dateKey = getCalendarDateKey(selectedCalendarDate.year, selectedCalendarDate.month, selectedCalendarDate.day);
            const tasks = getCalendarTasks(dateKey);
            
            if (tasks[index]) {
                tasks[index].completed = !tasks[index].completed;
                saveCalendarTasks(dateKey, tasks);
                loadTasksInPopup(tasks);
            }
        }

        function editCalendarTask(index, textElement) {
            if (!selectedCalendarDate) return;
            
            const originalText = textElement.textContent;
            const input = document.createElement('input');
            input.type = 'text';
            input.className = 'task-popup-input';
            input.value = originalText;
            input.style.margin = '0';
            
            textElement.style.display = 'none';
            textElement.parentElement.insertBefore(input, textElement);
            input.focus();
            input.select();
            
            function saveEdit() {
                if (input.value.trim()) {
                    const dateKey = getCalendarDateKey(selectedCalendarDate.year, selectedCalendarDate.month, selectedCalendarDate.day);
                    const tasks = getCalendarTasks(dateKey);
                    
                    if (tasks[index]) {
                        tasks[index].text = input.value.trim();
                        saveCalendarTasks(dateKey, tasks);
                        textElement.textContent = input.value.trim();
                    }
                }
                textElement.style.display = 'block';
                input.remove();
            }
            
            function cancelEdit() {
                textElement.style.display = 'block';
                input.remove();
            }
            
            input.addEventListener('blur', saveEdit);
            input.addEventListener('keydown', function(e) {
                if (e.key === 'Enter') {
                    saveEdit();
                } else if (e.key === 'Escape') {
                    cancelEdit();
                }
            });
        }

        function deleteCalendarTask(index) {
            if (!selectedCalendarDate) return;
            
            const dateKey = getCalendarDateKey(selectedCalendarDate.year, selectedCalendarDate.month, selectedCalendarDate.day);
            const tasks = getCalendarTasks(dateKey);
            
            tasks.splice(index, 1);
            saveCalendarTasks(dateKey, tasks);
            loadTasksInPopup(tasks);
            generateCircularCalendar(); // Refresh to update task dots
        }

        // Weekly Calendar Functions
        function getWeekStart(date) {
            const d = new Date(date);
            const day = d.getDay();
            const diff = d.getDate() - day + (day === 0 ? -6 : 1); // Monday as first day
            return new Date(d.setDate(diff));
        }

        function generateWeeklyCalendar() {
            const weekStart = getWeekStart(currentWeekStart);
            const weekEnd = new Date(weekStart);
            weekEnd.setDate(weekStart.getDate() + 6);
            
            // Update date range display
            const monthNames = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
                'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
            
            const startMonth = monthNames[weekStart.getMonth()];
            const endMonth = monthNames[weekEnd.getMonth()];
            const startDay = weekStart.getDate();
            const endDay = weekEnd.getDate();
            const year = weekStart.getFullYear();
            
            let dateRangeText;
            if (weekStart.getMonth() === weekEnd.getMonth()) {
                dateRangeText = `${startMonth} ${startDay} - ${endDay}, ${year}`;
            } else {
                dateRangeText = `${startMonth} ${startDay} - ${endMonth} ${endDay}, ${year}`;
            }
            
            document.getElementById('weeklyDateRange').textContent = dateRangeText;
            
            // Generate calendar grid
            const grid = document.getElementById('weeklyCalendarGrid');
            grid.innerHTML = '';
            
            const dayNames = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
            const today = new Date();
            today.setHours(0, 0, 0, 0);
            
            for (let i = 0; i < 7; i++) {
                const currentDay = new Date(weekStart);
                currentDay.setDate(weekStart.getDate() + i);
                
                const dayCard = document.createElement('div');
                dayCard.className = 'weekly-day-card';
                
                // Check if this is today
                const dayDate = new Date(currentDay);
                dayDate.setHours(0, 0, 0, 0);
                if (dayDate.getTime() === today.getTime()) {
                    dayCard.classList.add('today');
                }
                
                const dayKey = getWeeklyDateKey(currentDay);
                const tasks = getWeeklyTasks(dayKey);
                
                dayCard.innerHTML = `
                    <div class="weekly-day-header">
                        <div class="weekly-day-name">${dayNames[i]}</div>
                        <div class="weekly-day-number">${currentDay.getDate()}</div>
                    </div>
                    <div class="weekly-day-tasks" id="weekly-tasks-${i}">
                        <!-- Tasks will be loaded here -->
                    </div>
                    <button class="weekly-add-task" onclick="addWeeklyTask(${i})">+ Add task</button>
                `;
                
                grid.appendChild(dayCard);
                
                // Load tasks for this day
                loadWeeklyTasksForDay(i, tasks);
            }
        }

        function getWeeklyDateKey(date) {
            return `weekly_${date.getFullYear()}_${date.getMonth()}_${date.getDate()}`;
        }

        function getWeeklyTasks(dateKey) {
            const saved = localStorage.getItem(dateKey);
            return saved ? JSON.parse(saved) : [];
        }

        function saveWeeklyTasks(dateKey, tasks) {
            localStorage.setItem(dateKey, JSON.stringify(tasks));
        }

        function loadWeeklyTasksForDay(dayIndex, tasks) {
            const container = document.getElementById(`weekly-tasks-${dayIndex}`);
            if (!container) return;
            
            container.innerHTML = '';
            tasks.forEach((task, taskIndex) => {
                const taskElement = document.createElement('div');
                taskElement.className = 'weekly-task-item';
                taskElement.setAttribute('data-day', dayIndex);
                taskElement.setAttribute('data-task', taskIndex);
                
                taskElement.innerHTML = `
                    <div class="weekly-task-checkbox ${task.completed ? 'checked' : ''}" onclick="toggleWeeklyTask(${dayIndex}, ${taskIndex})"></div>
                    <div class="weekly-task-text ${task.completed ? 'completed' : ''}" onclick="editWeeklyTask(${dayIndex}, ${taskIndex}, this)">${task.text}</div>
                    <button class="weekly-task-delete" onclick="deleteWeeklyTask(${dayIndex}, ${taskIndex})">×</button>
                `;
                
                container.appendChild(taskElement);
            });
        }

        function addWeeklyTask(dayIndex) {
            showCustomPrompt('Enter task:', function(text) {
                if (text && text.trim()) {
                    const weekStart = getWeekStart(currentWeekStart);
                    const targetDate = new Date(weekStart);
                    targetDate.setDate(weekStart.getDate() + dayIndex);
                    
                    const dateKey = getWeeklyDateKey(targetDate);
                    const tasks = getWeeklyTasks(dateKey);
                    
                    tasks.push({
                        id: weeklyTaskIdCounter++,
                        text: text.trim(),
                        completed: false
                    });
                    
                    saveWeeklyTasks(dateKey, tasks);
                    loadWeeklyTasksForDay(dayIndex, tasks);
                }
            });
        }

        function toggleWeeklyTask(dayIndex, taskIndex) {
            const weekStart = getWeekStart(currentWeekStart);
            const targetDate = new Date(weekStart);
            targetDate.setDate(weekStart.getDate() + dayIndex);
            
            const dateKey = getWeeklyDateKey(targetDate);
            const tasks = getWeeklyTasks(dateKey);
            
            if (tasks[taskIndex]) {
                tasks[taskIndex].completed = !tasks[taskIndex].completed;
                saveWeeklyTasks(dateKey, tasks);
                loadWeeklyTasksForDay(dayIndex, tasks);
            }
        }

        function editWeeklyTask(dayIndex, taskIndex, textElement) {
            const originalText = textElement.textContent;
            
            showCustomPrompt(`Edit task: (Current: ${originalText})`, function(newText) {
                if (newText && newText.trim()) {
                    const weekStart = getWeekStart(currentWeekStart);
                    const targetDate = new Date(weekStart);
                    targetDate.setDate(weekStart.getDate() + dayIndex);
                    
                    const dateKey = getWeeklyDateKey(targetDate);
                    const tasks = getWeeklyTasks(dateKey);
                    
                    if (tasks[taskIndex]) {
                        tasks[taskIndex].text = newText.trim();
                        saveWeeklyTasks(dateKey, tasks);
                        loadWeeklyTasksForDay(dayIndex, tasks);
                    }
                }
            });
        }

        function deleteWeeklyTask(dayIndex, taskIndex) {
            const weekStart = getWeekStart(currentWeekStart);
            const targetDate = new Date(weekStart);
            targetDate.setDate(weekStart.getDate() + dayIndex);
            
            const dateKey = getWeeklyDateKey(targetDate);
            const tasks = getWeeklyTasks(dateKey);
            
            tasks.splice(taskIndex, 1);
            saveWeeklyTasks(dateKey, tasks);
            loadWeeklyTasksForDay(dayIndex, tasks);
        }

        function previousWeek() {
            currentWeekStart.setDate(currentWeekStart.getDate() - 7);
            generateWeeklyCalendar();
        }

        function nextWeek() {
            currentWeekStart.setDate(currentWeekStart.getDate() + 7);
            generateWeeklyCalendar();
        }

        // Vision Board Functions
        function loadVisionBoard() {
            const visionItems = JSON.parse(localStorage.getItem('visionBoard')) || [];
            const visionGrid = document.getElementById('visionGrid');
            
            visionGrid.innerHTML = '';
            
            // Create 9 vision board slots
            for (let i = 0; i < 9; i++) {
                const visionItem = document.createElement('div');
                visionItem.className = 'vision-item';
                visionItem.setAttribute('data-index', i);
                
                if (visionItems[i]) {
                    if (visionItems[i].type === 'image') {
                        visionItem.innerHTML = `
                            <img src="${visionItems[i].src}" alt="Vision item">
                            <div class="vision-item-overlay">
                                <button class="vision-overlay-btn" onclick="editVisionItem(${i})">Edit</button>
                                <button class="vision-overlay-btn" onclick="deleteVisionItem(${i})">Delete</button>
                            </div>
                        `;
                    } else {
                        visionItem.innerHTML = `
                            <span>${visionItems[i].text}</span>
                            <div class="vision-item-overlay">
                                <button class="vision-overlay-btn" onclick="editVisionItem(${i})">Edit</button>
                                <button class="vision-overlay-btn" onclick="deleteVisionItem(${i})">Delete</button>
                            </div>
                        `;
                    }
                } else {
                    visionItem.classList.add('empty');
                    visionItem.innerHTML = '+';
                    visionItem.onclick = () => addVisionItem(i);
                }
                
                visionGrid.appendChild(visionItem);
            }
        }

        function addVisionItem(index) {
            showCustomPrompt('Enter your vision/goal:', function(text) {
                if (text && text.trim()) {
                    const visionItems = JSON.parse(localStorage.getItem('visionBoard')) || [];
                    visionItems[index] = {
                        type: 'text',
                        text: text.trim()
                    };
                    localStorage.setItem('visionBoard', JSON.stringify(visionItems));
                    loadVisionBoard();
                }
            });
        }

        function editVisionItem(index) {
            const visionItems = JSON.parse(localStorage.getItem('visionBoard')) || [];
            const currentItem = visionItems[index];
            
            if (currentItem && currentItem.type === 'text') {
                showCustomPrompt(`Edit vision: (Current: ${currentItem.text})`, function(newText) {
                    if (newText && newText.trim()) {
                        visionItems[index].text = newText.trim();
                        localStorage.setItem('visionBoard', JSON.stringify(visionItems));
                        loadVisionBoard();
                    }
                });
            }
        }

        function deleteVisionItem(index) {
            const visionItems = JSON.parse(localStorage.getItem('visionBoard')) || [];
            visionItems.splice(index, 1);
            localStorage.setItem('visionBoard', JSON.stringify(visionItems));
            loadVisionBoard();
        }

        // Wellness Functions
        function updateWellnessItem(type) {
            const goals = {
                water: parseInt(document.getElementById('waterGoal').value) || 8,
                steps: parseInt(document.getElementById('stepsGoal').value) || 10000,
                sleep: parseFloat(document.getElementById('sleepGoal').value) || 8
            };
            
            const currentValues = getWellnessDataForDate(currentDate);
            
            let newValue;
            if (type === 'water') {
                newValue = Math.min((currentValues.water || 0) + 1, goals.water);
            } else if (type === 'steps') {
                const increment = Math.min(1000, goals.steps - (currentValues.steps || 0));
                newValue = Math.min((currentValues.steps || 0) + increment, goals.steps);
            } else if (type === 'sleep') {
                newValue = Math.min((currentValues.sleep || 0) + 0.5, goals.sleep);
            }
            
            currentValues[type] = newValue;
            saveWellnessDataForDate(currentDate, currentValues);
            updateWellnessDisplay();
        }

        function getWellnessDataForDate(date) {
            const dateKey = getDateKey(date);
            const saved = localStorage.getItem(`wellness_${dateKey}`);
            return saved ? JSON.parse(saved) : { water: 0, steps: 0, sleep: 0 };
        }

        function saveWellnessDataForDate(date, data) {
            const dateKey = getDateKey(date);
            localStorage.setItem(`wellness_${dateKey}`, JSON.stringify(data));
        }

        function updateWellnessDisplay() {
            const goals = {
                water: parseInt(document.getElementById('waterGoal').value) || 8,
                steps: parseInt(document.getElementById('stepsGoal').value) || 10000,
                sleep: parseFloat(document.getElementById('sleepGoal').value) || 8
            };
            
            const currentValues = getWellnessDataForDate(currentDate);
            
            // Update water
            const waterProgress = (currentValues.water / goals.water) * 100;
            const waterCircle = document.getElementById('waterProgress');
            const circumference = 157.08;
            const offset = circumference - (waterProgress / 100) * circumference;
            waterCircle.style.strokeDashoffset = offset;
            document.getElementById('waterValue').textContent = `${currentValues.water}/${goals.water} cups`;
            
            // Update steps
            const stepsProgress = (currentValues.steps / goals.steps) * 100;
            const stepsCircle = document.getElementById('stepsProgress');
            const stepsOffset = circumference - (stepsProgress / 100) * circumference;
            stepsCircle.style.strokeDashoffset = stepsOffset;
            document.getElementById('stepsValue').textContent = `${currentValues.steps}/${goals.steps} steps`;
            
            // Update sleep
            const sleepProgress = (currentValues.sleep / goals.sleep) * 100;
            const sleepCircle = document.getElementById('sleepProgress');
            const sleepOffset = circumference - (sleepProgress / 100) * circumference;
            sleepCircle.style.strokeDashoffset = sleepOffset;
            document.getElementById('sleepValue').textContent = `${currentValues.sleep}/${goals.sleep} hours`;
        }

        function updateWellnessGoal(type, value) {
            updateWellnessDisplay();
            localStorage.setItem(`${type}Goal`, value);
        }

        // Goals Management Functions
        function loadGoals() {
            const goals = JSON.parse(localStorage.getItem('goals')) || [];
            const goalsList = document.getElementById('goalsList');
            
            goalsList.innerHTML = '';
            goals.forEach(goal => {
                const goalItem = document.createElement('div');
                goalItem.className = 'goal-item';
                goalItem.setAttribute('data-goal-id', goal.id);
                
                let counterSection = '';
                let progressSection = '';
                
                if (goal.target && goal.target > 0) {
                    const progress = Math.min((goal.current / goal.target) * 100, 100);
                    
                    counterSection = `
                        <div class="goal-counter">
                            <button class="goal-counter-btn" onclick="decrementGoal('${goal.id}')" ${goal.current <= 0 ? 'disabled' : ''}>−</button>
                            <div class="goal-counter-value">${goal.current}/${goal.target}</div>
                            <button class="goal-counter-btn" onclick="incrementGoal('${goal.id}')" ${goal.current >= goal.target ? 'disabled' : ''}>+</button>
                        </div>
                    `;
                    
                    progressSection = `
                        <div class="goal-progress-container">
                            <div class="goal-progress-bar">
                                <div class="goal-progress-fill" style="width: ${progress}%"></div>
                            </div>
                            <div class="goal-progress-text">${Math.round(progress)}% complete</div>
                        </div>
                    `;
                } else {
                    counterSection = `
                        <div class="checkbox ${goal.completed ? 'checked' : ''}" onclick="toggleGoalCompletion('${goal.id}')"></div>
                    `;
                }
                
                goalItem.innerHTML = `
                    <div class="goal-content">
                        <div class="goal-main-row">
                            <div class="goal-text ${goal.completed ? 'completed' : ''}" onclick="editGoal('${goal.id}', this)">${goal.text}</div>
                            ${counterSection}
                        </div>
                        ${progressSection}
                    </div>
                    <button class="delete-btn" onclick="deleteGoal('${goal.id}')">×</button>
                `;
                
                goalsList.appendChild(goalItem);
            });
        }

        function showAddGoalForm() {
            document.getElementById('addGoalForm').style.display = 'block';
            document.getElementById('goalInput').focus();
        }

        function cancelAddGoal() {
            document.getElementById('addGoalForm').style.display = 'none';
            document.getElementById('goalInput').value = '';
            document.getElementById('goalTargetInput').value = '';
        }

        function saveGoal() {
            const goalInput = document.getElementById('goalInput');
            const targetInput = document.getElementById('goalTargetInput');
            
            if (goalInput.value.trim()) {
                const goals = JSON.parse(localStorage.getItem('goals')) || [];
                const target = parseInt(targetInput.value) || 0;
                
                const newGoal = {
                    id: 'goal_' + Date.now(),
                    text: goalInput.value.trim(),
                    target: target,
                    current: 0,
                    completed: false
                };
                
                goals.push(newGoal);
                localStorage.setItem('goals', JSON.stringify(goals));
                
                cancelAddGoal();
                loadGoals();
                showToast('Goal added successfully!');
            }
        }

        function incrementGoal(goalId) {
            const goals = JSON.parse(localStorage.getItem('goals')) || [];
            const goal = goals.find(g => g.id === goalId);
            
            if (goal && goal.current < goal.target) {
                goal.current++;
                if (goal.current >= goal.target) {
                    goal.completed = true;
                    showToast('🎉 Goal completed! Congratulations!');
                }
                localStorage.setItem('goals', JSON.stringify(goals));
                loadGoals();
            }
        }

        function decrementGoal(goalId) {
            const goals = JSON.parse(localStorage.getItem('goals')) || [];
            const goal = goals.find(g => g.id === goalId);
            
            if (goal && goal.current > 0) {
                goal.current--;
                if (goal.completed && goal.current < goal.target) {
                    goal.completed = false;
                }
                localStorage.setItem('goals', JSON.stringify(goals));
                loadGoals();
            }
        }

        function toggleGoalCompletion(goalId) {
            const goals = JSON.parse(localStorage.getItem('goals')) || [];
            const goal = goals.find(g => g.id === goalId);
            
            if (goal) {
                goal.completed = !goal.completed;
                if (goal.completed) {
                    showToast('🎉 Goal completed! Great job!');
                }
                localStorage.setItem('goals', JSON.stringify(goals));
                loadGoals();
            }
        }

        function editGoal(goalId, textElement) {
            if (editingTodo) return;
            
            editingTodo = textElement;
            const originalText = textElement.textContent;
            
            const input = document.createElement('input');
            input.type = 'text';
            input.className = 'edit-input';
            input.value = originalText;
            
            textElement.style.display = 'none';
            textElement.parentElement.insertBefore(input, textElement);
            input.focus();
            input.select();
            
            function saveEdit() {
                if (input.value.trim()) {
                    const goals = JSON.parse(localStorage.getItem('goals')) || [];
                    const goal = goals.find(g => g.id === goalId);
                    
                    if (goal) {
                        goal.text = input.value.trim();
                        localStorage.setItem('goals', JSON.stringify(goals));
                        textElement.textContent = input.value.trim();
                    }
                }
                textElement.style.display = 'block';
                input.remove();
                editingTodo = null;
            }
            
            function cancelEdit() {
                textElement.style.display = 'block';
                input.remove();
                editingTodo = null;
            }
            
            input.addEventListener('blur', saveEdit);
            input.addEventListener('keydown', function(e) {
                if (e.key === 'Enter') {
                    saveEdit();
                } else if (e.key === 'Escape') {
                    cancelEdit();
                }
            });
        }

        function deleteGoal(goalId) {
            const goals = JSON.parse(localStorage.getItem('goals')) || [];
            const updatedGoals = goals.filter(g => g.id !== goalId);
            localStorage.setItem('goals', JSON.stringify(updatedGoals));
            loadGoals();
            showToast('Goal deleted successfully!');
        }

        // Initialize App
        document.addEventListener('DOMContentLoaded', function() {
            loadTheme();
            generateCalendar();
            loadTasksForDate(new Date());
            loadDailyPriorities();
            loadHabits();
            loadRoutines();
            loadVisionBoard();
            loadGoals();
            generateCircularCalendar();
            generateWeeklyCalendar();
            updateWellnessDisplay();
            
            // Set current week start to this week's Monday
            currentWeekStart = getWeekStart(new Date());
        });
    </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a5c15c5e67716b4',t:'MTc2NDM1NjE4NC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
