% ============================================
% REAL-TIME HX711 Load Cell Plot in MATLAB
% Using Arduino Serial Output
% ============================================

clear; clc; close all;

% ---- 1. Connect to Arduino on COM8 ----
s = serialport("COM8", 9600, "Timeout", 2);
flush(s);

disp("Connected to COM8");
disp("Collecting data... press Ctrl-C to stop.");

% ---- 2. Setup Plot ----
figure;
h = animatedline('LineWidth', 2);
xlabel("Time (s)");
ylabel("Weight (kg)");
title("Real-Time Load Cell Reading (HX711)");

startTime = tic;
timeData = [];
weightData = [];

% ---- 3. Real-time loop ----
while true
    if s.NumBytesAvailable > 0
        raw = readline(s);      % read line from Arduino
        raw = strtrim(raw);     % remove spaces/newlines
        disp(raw);              % show raw line in MATLAB console
        
        % Extract numeric weight using regex
        tokens = regexp(raw, 'Reading:\s*([-+]?\d*\.?\d+)', 'tokens');
        
        if ~isempty(tokens)
            weight = str2double(tokens{1}{1});
            t = toc(startTime);

            addpoints(h, t, weight);
            drawnow limitrate;

            timeData(end+1) = t;
            weightData(end+1) = weight;
        end
    end
end
