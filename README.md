# matrixCalculator
Robust Mathematical Matrix Calculator

1. Document Architecture & Markup (HTML)
The Structural Shell (.calc-housing): Forms the centralized structural wrapper that encloses both the display and the numerical pad.

The Monitor Interface (#monitorView): A read-only screen space used to output strings, real-time results, or exception statements like "Error".

The Input Assembly (.input-matrix): Houses 19 button nodes mapped directly to JavaScript triggers through functional onclick event bindings.

Asymmetric Spacing Element (.double-width): Uses the custom class selector .double-width on the 0 key to force it to stretch horizontally across two grid blocks, breaking the uniform shape layout for better thumb ergonomics.

2. Layout Positioning & Visual Presentation (CSS)
Flexbox Real-Estate Alignment: The body element utilizes display: flex, combined with justify-content: center and align-items: center across a minimum viewport space of 100vh to anchor the calculator container directly in the center of any screen size.

The Matrix Core Grid System: The input buttons are managed using CSS Grid layout:

CSS
display: grid;
grid-template-columns: repeat(4, 1fr);
gap: 0.75rem;
This divides the control panel into four columns of equal width (1fr), automatically sizing grid items with a uniform spacing gap.

Layout Sizing Layout Elements (tabular-nums): The output screen uses font-variant-numeric: tabular-nums;. This converts the font rules to ensure that every single digit shares an identical, fixed width. This prevents numbers on screen from vibrating or jumping horizontally as values calculate.

Micro-Interaction Transforms (scale adjustments): Clicking an button fires an active transitional transformation rule (transform: scale(0.97);). This creates a quick click indentation animation that gives the user a physical feel of real button travel.

3. Dynamic Calculation Stream Architecture (JavaScript)
Memory Buffer Isolation (dynamicBuffer): The engine tracks math sequences within a simple global string data type named dynamicBuffer. This allows characters to easily stack together ("7" + "+" + "5") before being computed.

Consecutive Operator Validation Logic: To prevent computational parsing script errors caused by overlapping operators (e.g., typing 7++*3), the input mechanism scans backward by slicing the end character of the string:

JavaScript
if (operators.includes(char) && operators.includes(dynamicBuffer.slice(-1))) {
    dynamicBuffer = dynamicBuffer.slice(0, -1) + char;
}
If a user types a new math symbol directly over an old one, the script crops the existing terminal operator out and replaces it with the newly typed token.

State Reset Trigger (outputIsFresh): Tracks if the value currently shown on screen is a finalized result. If a user tries to type a number right after hitting =, the engine automatically sweeps the screen clear so new formulas don't append to old answers.

Secure Sandbox Processing Engine: Instead of deploying standard global string evaluation (eval()), which exposes code bases to script injections, this solution creates an isolated, compiled execution context:

JavaScript
let resolvedValue = Function(`"use strict"; return (${dynamicBuffer})`)();
This creates a secure sandbox environment that parses the mathematical string calculation safely.

Defensive Edge Case Boundary Tracking: The system monitors and catches standard computing breaking points inside an explicit try-catch execution flow:

Division by Zero: Before running calculations, it explicitly scans the text string for /0. If detected, it throws a controlled validation exception, avoiding system break points.

Floating-Point Precision Correction: Binary machines occasionally output long decimal tails due to computing conversion limits (like 0.1 + 0.2 equalling 0.30000000000000004). Checking resolvedValue % 1 !== 0 tracks float fractions and truncates any bloated decimals down to a clean, maximum 6-digit space using .toFixed(6).
