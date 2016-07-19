---
layout: post
title: "matlab小结"
date: 2016-07-19 18:41:57 +0800
comments: true
categories: matlab
---

最近，有做一个图像处理的项目，总结出一些有用的maltlab小工具。

1. [Numsubplot](#1)
2. [windRose](#2)
3. [多项式拟合](#3)
4. [箭头](#4)
5. [误差带](#5)
6. [matlab编程配置](#6)
7. [cell and array](#7)
<!--more-->
<h2 id="1">1. Numsubplot</h2>

在进行分析的时候，有时候可能需要多图打印，numsubplot还是挺方便的。使用可以看注释部分
``` matlab
    function [p,n]=numSubplots(n)
% function [p,n]=numSubplots(n)
    %
    % Purpose
    % Calculate how many rows and columns of sub-plots are needed to
    % neatly display n subplots. 
    %
    % Inputs
    % n - the desired number of subplots.     
    %  
    % Outputs
    % p - a vector length 2 defining the number of rows and number of
    %     columns required to show n plots.     
    % [ n - the current number of subplots. This output is used only by
    %       this function for a recursive call.]
    %
    %
    %
    % Example: neatly lay out 13 sub-plots
% >> p=numSubplots(13)
    % p = 
    %     3   5
    % for i=1:13; subplot(p(1),p(2),i), pcolor(rand(10)), end 
    %
    %
    % Rob Campbell - January 2010


    while isprime(n) & n>4, 
    n=n+1;
end

p=factor(n);

if length(p)==1
p=[1,p];
return
end


while length(p)>2
if length(p)>=4
p(1)=p(1)*p(end-1);
p(2)=p(2)*p(end);
p(end-1:end)=[];
else
p(1)=p(1)*p(2);
p(2)=[];
end    
p=sort(p);
end


%Reformat if the column/row ratio is too large: we want a roughly
%square design 
while p(2)/p(1)>2.5
N=n+1;
[p,n]=numSubplots(N); %Recursive!
end

```

<h2 id="2">2. Winrose</h2>

a. 可以用于绘制风玫瑰图，也就是风向变量和风速变量
b. 也可以用于绘制单一变量的玫瑰图，比如角度玫瑰图

``` matlab
function [figure_handle,colorSeq,barX,barY,count,speeds,directions,Table] = WindRose(direction,speed,varargin)
    %  WindRose  Draw a Wind Rose knowing direction and speed
    %
    %  [figure_handle,count,speeds,directions,Table] = WindRose(direction,speed);
    %  [figure_handle,count,speeds,directions,Table] = WindRose(direction,speed,'parameter1',value1,...);
    %
    %  figure_handle is the handle of the figure containing the chart
    %  count is the frequency for each speed (ncolumns = nspeeds) and for each direction (nrows = ndirections).
    %  speeds is a 1 by n vector containing the values for the speed intervals
    %  directions is a m by 1 vector containing the values in which direction intervals are centered
    %  Table is a (4+m) by (3+m) cell array (excel-ready), containing Frequencies for each direction and for each speed. 
    %
    %  User can specify direction angle meaning North and East winds, so
    %  the graphs is shown in the desired reference
    %
    %     Example
    %     d = 360 * rand(10000,1); % My reference is North = 0? East = 90?
    %     v = 30*rand(10000,1);
    % 
    %     [figure_handle,count,speeds,directions,Table] = WindRose(d,v,'anglenorth',0,'angleeast',90,'freqlabelangle',45);
    %
    % PARAMETER          CLASS         DEFAULT VALUE         DESCRIPTION
    %------------------------------------------------------------------------------------------------------------------------------------------------------------
    % 'centeredin0'      Logical.      true                  Is first angular bin centered in 0 (-5 to 5)? -> CeteredIn0 = true // or does it start in 0 (0 to 10)? -> CeteredIn0 = false.
    % 'ndirections'      Numeric.      36                    Number of direction bins (subdivisions) to be shown.
    % 'freqround'        Numeric.      1                     Maximum frquency value will be rounded to the first higher whole multiple of FrequenciesRound. Only applies if 'maxfrequency' is specified.
    % 'nfreq'            Numeric.      5                     Draw this number of circles indicating frequency.
    % 'speedround'       Numeric.      [] (auto)             Maximum wind speed will be rounded to the first higher whole multiple of WindSpeedRound.
    % 'nspeeds'          Numeric.      [] (auto)             Draw this number of windspeeds subdivisions (bins). Default is 6 if 'speedround' is specified. Otherwise, default is automatic.
    % 'maxfrequency'     Numeric.      [] (auto) or 6        Set the value of the maximum frequency circle to be displayed. Be careful, because bins radius keep the original size.
    % 'freqlabelangle'   Numeric.      [] (auto)             Angle in which frequecy labels are shown. If this value is empty, frequency labels will NOT be shown. Trigonometric reference. 0=Right, 90=Up.
    % 'titlestring'      Cell/String.  {'Wind Rose';' '}     Figure title. It is recommended to include an empty line below the main string.
    % 'lablegend'        String.       'Wind speeds in m/s'  String that will appear at the top of the legend. Can be empty.
    % 'cmap'             String.       'jet'                 String with the name of a colormap function. If you put inv before the name of the funcion, colors will be flipped (e.g. 'invjet', 'invautum', 'invbone', ...). Use any of the built-in functions (autumn, bone, colorcube, cool, copper, flag, gray, hot, hsv, jet, lines, pink, prism, spring, summer, white, winter). See doc colormap for more information.
    % 'height'           Numeric.      2/3*screensize        Figure inner height in pixels. Default is 2/3 of minimum window dimension;
    % 'width'            Numeric.      2/3*screensize        Figure inner width in pixels.
    % 'figcolor'         Color Code.   'w'                   Figure color, any admitted matlab color format. Default is white.
    % 'textcolor'        Color Code.   'k'                   Text and axis color, any admitted matlab color format. Default is black.
    % 'labels'           CellString    {'N','S','E','W'}     Specify North South East West in a cell array of strings.
    % 'labelnorth'       String.       'N'                   Label to indicate North. Be careful if you specify 'labels' and 'labelnorth'. Last parameter specified will be the one used.
    % 'labelsouth'       String.       'S'                   Label to indicate South. Be careful if you specify 'labels' and 'labelsouth'. Last parameter specified will be the one used.
    % 'labeleast'        String.       'E'                   Label to indicate East.  Be careful if you specify 'labels' and 'labeleast' . Last parameter specified will be the one used.
    % 'labelwest'        String.       'W'                   Label to indicate West.  Be careful if you specify 'labels' and 'labelwest' . Last parameter specified will be the one used.
    % 'titlefontweight'  String.       'bold'                Title font weight. You can use 'normal','bold','light','demi'
    % 'legendvariable'   String.       'W_S'                 Variable abbreviation that appears inside the legend. You can use TeX sequences.
    % 'anglenorth'       Numeric.       90                   Direction angle meaning North wind. Default is 90 for North (trigonometric reference). If you specify 'north' angle, you need to specify 'east' angle, so the script knows how angles are referenced.
    % 'angleeast'        Numeric.       0                    Direction angle meaning East wind.  Default is  0 for East  (counterclockwise).        If you specify 'east' angle, you need to specify 'north' angle, so the script knows how angles are referenced.
    % 'minradius'        Numeric        1/30                 Minimum radius of the wind rose, relative to the maximum frequency radius. An empty circle of this size appears if greater than 0.
    % 'legendtype'       Numeric        2                    Legend type continuous = 1, separated boxes = 2
    %
    %
    % by Daniel Pereira - dpereira@s2msolutions.com
    %
    % 2014/Jul/14 - First version

    %% Check funciton call
    if nargin<2
    error('WindRose needs at least two inputs');
    elseif mod(length(varargin),2)~=0
    error('Inputs must be paired: WindRose(Speed,Direction,''PropertyName'',PropertyValue,...)');
elseif ~isnumeric(speed) || ~isnumeric(speed)
    error('Speed and Direction must be numeric arrays.');
elseif ~isequal(size(speed),size(direction))
    error('Speed and Direction must be the same size.');
    end

    %% Default parameters
    SCS              = get(0,'screensize');

    CeteredIn0       = true;
    ndirections      = 36;
    FrequenciesRound = 1;
    NFrequencies     = 5;
    WindSpeedRound   = [];
    NSpeeds          = [];
    circlemax        = [];
    FreqLabelAngle   = [];
    TitleString      = {'Wind Rose';' '};
%lablegend        = 'Wind Speeds in m/s';
lablegend        = 'flow angle in °';
colorfun         = 'jet';
height           = min(SCS(3:4))*2/3;
width            = min(SCS(3:4))*2/3;
figcolor         = '[1,1,1]';
TextColor        = '[0,0,0]';
% label.N          = 'N';
% label.S          = 'S';
% label.W          = 'W';
% label.E          = 'E';
label.N          = '90°';
label.S          = '270°';
label.W          = '180°';
label.E          = '0°';
titlefontweight  = 'bold';
legendvariable   = 'W_S';
RefN             = 90;
RefE             = 0;
min_radius       = 1/30;
LegendType       = 2;

%% User-.specified parameters

    for i=1:2:numel(varargin)
switch lower(varargin{i})
    case 'centeredin0'
    CeteredIn0       = varargin{i+1};
case 'ndirections'
ndirections      = varargin{i+1};
case 'freqround'
FrequenciesRound = varargin{i+1};
case 'nfreq'
NFrequencies     = varargin{i+1}; 
case 'speedround'
WindSpeedRound   = varargin{i+1};
case 'nspeeds'
NSpeeds          = varargin{i+1};
case 'freqlabelangle'
FreqLabelAngle   = varargin{i+1};
case 'titlestring'
TitleString      = varargin{i+1};
case 'lablegend'
lablegend        = varargin{i+1};
case 'cmap'
colorfun         = varargin{i+1};
case 'height'
height           = varargin{i+1};
case 'width'
width            = varargin{i+1};
case 'figcolor'
figcolor         = varargin{i+1};
case 'textcolor'
TextColor        = varargin{i+1};
case 'min_radius'
min_radius       = varargin{i+1};
case 'maxfrequency'
circlemax = varargin{i+1};
case 'titlefontweight'
titlefontweight  = varargin{i+1};
case 'legendvariable'
legendvariable   = varargin{i+1};
case 'legendtype'
LegendType       = varargin{i+1};
case 'labelnorth'
label.N          = varargin{i+1};
case 'labelsouth'
label.S          = varargin{i+1};
case 'labeleast'
label.E          = varargin{i+1};
case 'labelwest'
label.W          = varargin{i+1};
case 'labels'
label.N          = varargin{i+1}{1};
label.S          = varargin{i+1}{2};
label.E          = varargin{i+1}{3};
label.W          = varargin{i+1}{4};
case 'anglenorth'
k = any(arrayfun(@(x) strcmpi(x,'angleeast'),varargin));
if ~k
error('Reference angles need to be specified for AngleEAST and AngleNORTH directions');
end
case 'angleeast'
k = find(arrayfun(@(x) strcmpi(x,'anglenorth'),varargin));
if isempty(k)
    error('Reference angles need to be specified for AngleEAST and AngleNORTH directions');
    else
    RefE         = varargin{i+1};
RefN         = varargin{k+1};
end
if abs(RefN-RefE)~=90
error('The angles specified for north and east must differ in 90 degrees');
end
otherwise
error([varargin{i} ' is not a valid property for WindRose function.']);
end
end

speed            = reshape(speed,[],1);                                    % Convert wind speed into a column vector
direction        = reshape(direction,[],1);                                % Convert wind direction into a column vector
NumberElements   = numel(direction);                                       % Coun the actual number of elements, to consider winds = 0 when calculating frequency.
dir              = mod((RefN-direction)/(RefN-RefE)*90,360);               % Ensure that the direction is between 0 and 360?
speed            = speed(speed>0);                                         % Only show winds higher than 0. 縒hy? See next comment.
dir              = dir(speed>0);                                           % Wind = 0 does not have direction, so it cannot appear in a wind rose, but the number of appeareances must be considered.

figure_handle = figure('color',figcolor,'units','pixels','position',[SCS(3)/2-width/2 SCS(4)/2-height/2 width height],'menubar','none','toolbar','none');
%% Bin Directions
N     = linspace(0,360,ndirections+1);                                     % Create ndirections direction intervals (ndirections+1 edges)
N     = N(1:end-1);                                                        % N is the angles in which direction bins are centered. We do not want the 360 to appear, because 0 is already appearing.
n     = 180/ndirections;                                                   % Angle that should be put backward and forward to create the angular bin, 1st centered in 0
if ~CeteredIn0                                                             % If user does not want the 1st bin to be centered in 0?
N = N+n;                                                               % Bin goes from 0 to 2n (N to N+2n), instead of from -n to n (N-n to N+n), so Bin is not centered in 0 (N) angle, but in the n (N+n) angle
end

    %% Wind speeds/velocities
if ~isempty(WindSpeedRound)
    if isempty(NSpeeds); NSpeeds = 6; end                                  % Default value for NSpeeds if not user specified
    vmax      = ceil(max(speed)/WindSpeedRound)*WindSpeedRound;            % Max wind speed rounded to the nearest whole multiple of WindSpeedRound (Use round or ceil as desired)
    if vmax==0; vmax=WindSpeedRound; end;                      % If max wind speed is 0, make max wind to be WindSpeedRound, so wind speed bins are correctly shown.
    vwinds    = linspace(0,vmax,NSpeeds);                                  % Wind speeds go from 0 to vmax, creating the desired number of wind speed intervals
    else
    figure2 = figure('visible','off'); plot(speed);                        % Plot wind speed
    vwinds = get(gca,'ytick'); delete(figure2);                            % Yaxis will automatically make divisions or us.
if ~isempty(NSpeeds)
    vwinds = linspace(min(vwinds),max(vwinds),NSpeeds);
    end
    end

    %% Histogram in each direction + Draw
    count     = PivotTableCount(N,n,vwinds,speed,dir,NumberElements);          % For each direction and for each speed, value of the radius that the windorose must reach (Accumulated in speed).

if isempty(circlemax)
    circlemax = ceil(max(max(count))/FrequenciesRound)*FrequenciesRound;   % Round highest frequency to closest whole multiple of theFrequenciesRound  (Use round or ceil as desired)
    end
    min_radius = min_radius*circlemax;

    DrawPatches(N,n,vwinds,count,colorfun,figcolor,min_radius);% Draw the windrose, knowing the angles, the range for each direction, the speed ranges, the count (frequency) values and the colormap used.

    %% Constant frequecy circles and x-y axes + Draw + Labels

    [x,y]     = cylinder(1,50); x = x(1,:); y = y(1,:);                        % Get x and y for a unit-radius circle
    circles   = linspace(0,circlemax,NFrequencies+1); circles = circles(2:end);% Radii of the circles that must be drawn (frequencies). We do not want to spend time drawing radius=0.

    radius    = circles   + min_radius;
    radiusmax = circlemax + min_radius;

    plot(x'*radius,y'*radius,':','color',TextColor);                           % Draw circles
    plot(x*radiusmax,y*radiusmax,'-','color',TextColor);                       % Redraw last circle

    axisangles = 0:30:360; axisangles = axisangles(1:end-1);                   % Angles in which to draw the radial axis (trigonometric reference)
    R = [min_radius;radiusmax];
    plot(R*cosd(axisangles),R*sind(axisangles),':','color',TextColor);         % Draw radial axis, in the specified angles

    FrequecyLabels(circles,radius,FreqLabelAngle,TextColor);                   % Display frequency labels
    CardinalLabels(radiusmax,TextColor,label);                                 % Display N, S, E, W

    %% Title and Legend
    title(TitleString,'color',TextColor,'fontweight',titlefontweight);         % Display a title
    set(gca,'outerposition',[0 0 1 1]);                                        % Check that the current axis fills the figure.
    if LegendType==2
    leyenda = CreateLegend(vwinds,lablegend,legendvariable);               % Create a legend cell string
    l       = legend(leyenda,'location','southwest');                      % Display the legend wherever (position is corrected)
    PrettyLegend(l,TextColor);                                   % Display the legend in a good position
    elseif LegendType==1
    disp(vwinds);
    caxis([vwinds(1) vwinds(end)]);
    colorbar('YTick',vwinds);
    end

    %% Outputs
    [count,speeds,directions,Table,colorSeq,barX,barY] = CreateOutputs(count,vwinds,N,n,RefN,RefE);

function count = PivotTableCount(N,n,vwinds,speed,dir,NumberElements)
    count  = zeros(length(N),length(vwinds));
for i=1:length(N)
    d1 = mod(N(i)-n,360);                                              % Direction 1 is N-n
    d2 = N(i)+n;                                                       % Direction 2 is N+n
if d1>d2                                                           % If direction 1 is greater than direction 2 of the bin (d1 = -5 = 355, d2 = 5)
    cond = or(dir>=d1,dir<d2);                                     % The condition is satisfied whenever d>=d1 or d<d2
    else                                                               % For the rest of the cases,
    cond = and(dir>=d1,dir<d2);                                    % Both conditions must be met for the same bin
    end
    counter    = histc(speed(cond),vwinds);                            % If vmax was for instance 25, counter will have counts for these intervals: [>=0 y <5] [>=5 y <10] [>=10 y <15] [>=15 y <20] [>=20 y <25] [>=25]
    if isempty(counter); counter = zeros(1,size(count,2)); end         % If counter is empty for any reason, set the counts to 0.
    count(i,:) = cumsum(counter);                                      % Computing cumsum will make count to have the counts for [<5] [<10] [<15] [<20] [<25] [>=25] (cumulative count, so we have the radius for each speed)
    end
    count = count/NumberElements*100;                                      % Frequency in percentage

function DrawPatches(N,n,vwinds,count,colorfun,figcolor,min_radius)
    inv = strcmp(colorfun(1:3),'inv');                                     % INV = First three letters in cmap are inv
    if inv; colorfun = colorfun(4:end); end                                % if INV, cmap is the rest, excluding inv
    color = feval(colorfun,256);                                           % Create color map
    color = interp1(linspace(1,length(vwinds),256),color,1:length(vwinds));% Get the needed values.
    if inv; color = flipud(color); end;                                    % if INV, flip upside down the colormap
    plot(0,0,'.','color',figcolor,'markeredgecolor',figcolor,'markerfacecolor',figcolor); % This will create an empty legend entry.
    hold on; axis square; axis off;
for i=1:length(N)
    for j=length(vwinds):-1:1
                          if j>1
                          r(1) = count(i,j-1);
                          else
                          r(1) = 0;                                                  % For the first case, radius is 0
                          end
                          r(2)  = count(i,j);
                          r     = r+min_radius;

                          alpha = linspace(-n,n,100)+N(i);
                          x1    = r(1) * sind(fliplr(alpha));
                          y1    = r(1) * cosd(fliplr(alpha));
                          x     = [x1 r(2)*sind(alpha)];                           % Create circular sectors
                          y     = [y1 r(2)*cosd(alpha)];
                          fill(x,y,color(j,:),'edgecolor',hsv2rgb(rgb2hsv(color(j,:)).*[1 1 0.7])); % Draw them in the specified coloe. Edge is slightly darker.
                          %text(x(1),y(1),num2str(count(i,j)));  %% add label
                          end
                          end

function FrequecyLabels(circles,radius,angulo,TextColor)
    s = sind(angulo); c = cosd(angulo);                                      % Get the positions in which labels must be placed
    if c>0; ha = 'left';   elseif c<0; ha = 'right'; else ha = 'center'; end % Depending on the sign of the cosine, horizontal alignment should be one or another
    if s>0; va = 'bottom'; elseif s<0; va = 'top';   else va = 'middle'; end % Depending on the sign of the sine  , vertical   alignment should be one or another
for i=1:length(circles)
    text(radius(i)*c,radius(i)*s,[num2str(circles(i)) '%'],'HorizontalAlignment',ha,'verticalalignment',va,'color',TextColor); % display the labels for each circle
    end
    rmin = radius(1)-abs(diff(radius(1:2)));
    if rmin>0
    if c>0; ha = 'right'; elseif c<0; ha = 'left';   else ha = 'center'; end % Depending on the sign of the cosine, horizontal alignment should be one or another
    if s>0; va = 'top';   elseif s<0; va = 'bottom'; else va = 'middle'; end % Depending on the sign of the sine  , vertical   alignment should be one or another
    text(rmin*c,rmin*s,'0%','HorizontalAlignment',ha,'verticalalignment',va,'color',TextColor); % display the labels for each circle
    end

function CardinalLabels(circlemax,TextColor,labels)
    text( circlemax,0,[' ' labels.E],'HorizontalAlignment','left'  ,'verticalalignment','middle','color',TextColor); % East  label
    text( circlemax*cos(30*pi/180),circlemax*sin(30*pi/180),[' 30°'],'HorizontalAlignment','left'  ,'verticalalignment','middle','color',TextColor); % East  label
    text( circlemax*cos(60*pi/180),circlemax*sin(60*pi/180),[' 60°'],'HorizontalAlignment','left'  ,'verticalalignment','middle','color',TextColor); 
    text( (circlemax+3)*cos(120*pi/180),(circlemax+3)*sin(120*pi/180),['120°'],'HorizontalAlignment','left'  ,'verticalalignment','middle','color',TextColor); 
    text( (circlemax+4)*cos(150*pi/180),(circlemax+4)*sin(150*pi/180),['150°'],'HorizontalAlignment','left'  ,'verticalalignment','middle','color',TextColor); 
    text( (circlemax+4)*cos(210*pi/180),(circlemax+4)*sin(210*pi/180),['210°'],'HorizontalAlignment','left'  ,'verticalalignment','middle','color',TextColor); 
    text( (circlemax+3)*cos(240*pi/180),(circlemax+3)*sin(240*pi/180),['240°'],'HorizontalAlignment','left'  ,'verticalalignment','middle','color',TextColor);     
    text( circlemax*cos(300*pi/180),circlemax*sin(300*pi/180),[' 300°'],'HorizontalAlignment','left'  ,'verticalalignment','middle','color',TextColor); 
    text( circlemax*cos(330*pi/180),circlemax*sin(330*pi/180),[' 330°'],'HorizontalAlignment','left'  ,'verticalalignment','middle','color',TextColor); 
    text(-circlemax,0,[labels.W ' '],'HorizontalAlignment','right' ,'verticalalignment','middle','color',TextColor); % West  label
    text(0, circlemax,labels.N      ,'HorizontalAlignment','center','verticalalignment','bottom','color',TextColor); % North label
    text(0,-circlemax,labels.S      ,'HorizontalAlignment','center','verticalalignment','top'   ,'color',TextColor); % South label
    xlim([-circlemax circlemax]);
    ylim([-circlemax circlemax]);

function leyenda = CreateLegend(vwinds,lablegend,legendvariable)
    leyenda = cell(length(vwinds),1);                                      % Initialize legend cell array
for j=1:length(vwinds)
    if j==length(vwinds)                                               % When last index is reached
    string = sprintf('%s %s %g',legendvariable,'\geq',vwinds(j));  % Display wind <= max wind
    else                                                               % For the rest of the indices
    string = sprintf('%g %s %s < %g',vwinds(j),'\leq',legendvariable,vwinds(j+1)); % Set v1 <= v2 < v1
    end
    string = regexprep(string,'0 \leq','0 <');                         % Replace "0 <=" by "0 <", because wind speed = 0 is not displayed in the graph.
    leyenda{length(vwinds)-j+1} = string;
    end
    leyenda = [lablegend; leyenda];                                        % Add the title for the legend

function PrettyLegend(l,TextColor)
    set(l,'units','normalized','box','off');                               % Do not display the box
    POS = get(l,'position');                                               % get legend position (width and height)
    set(l,'position',[0 1-POS(4) POS(3) POS(4)],'textcolor',TextColor);    % Put the legend in the upper left corner
    uistack(l,'bottom');                                                   % Put the legend below the axis

function [count,speeds,directions,Table,colorSeq,barX,barY] = CreateOutputs(count,vwinds,N,n,RefN,RefE)
    count          = [count(:,1) diff(count,1,2)];                         % Count had the accumulated frequencies. With this line, we get the frequency for each single direction and each single speed with no accumulation.
    speeds         = vwinds;                                               % Speeds are the same as the ones used in the Wind Rose Graph
    directions     = mod(RefN - N'/90*(RefN-RefE),360);                    % Directions are the directions in which the sector is centered. Convert function reference to user reference
    vwinds(end+1)  = inf;                                                  % Last wind direction is inf (for creating intervals)

    [directions,i] = sort(directions);                                     % Sort directions in ascending order
    count          = count(i,:);                                           % Sort count in the same way.

    wspeeds        = cell(1,length(vwinds)-1);
for i=1:(length(vwinds)-1)
    if vwinds(i) == 0; s1 = '('; else s1 = '['; end                     % If vwinds(i) =0 interval is open, because count didn't compute windspeed = 0.
    wspeeds{i} = [s1 num2str(vwinds(i)) ' , ' num2str(vwinds(i+1)) ')'];% Create wind speed intervals
    end

    wdirs = cell(length(directions),1);
for i=1:length(directions)
    wdirs{i} = sprintf('[%g , %g)',mod(directions(i)-n,360),directions(i)+n); % Create widn direction intervals
    end

    %     countMark         = find(count == max(max(count)));
    %     countColumnLength = length(directions);
    %     foundIndex        = countMark -countColumnLength * floor(countMark/countColumnLength);
    %     colorSeq          = [mod(directions(foundIndex)-n,360),directions(foundIndex)+n];
    WindZeroFreqency = 100-sum(sum(count));                                % Wind speed = 0 appears 100-sum(total) % of the time. It does not have direction.
    WindZeroFreqency = WindZeroFreqency*(WindZeroFreqency/100>eps);        % If frequency/100% is lower than eps, do not show that value.

    Table            = [{'Frequencies (%)'},{''},{'Wind Speed Interval'},repmat({''},1,numel(wspeeds));'Direction Interval (?','Direction ~',wspeeds,'TOTAL';[wdirs num2cell(directions) num2cell(count) num2cell(sum(count,2))]]; % Create table cell. Ready to xlswrite.
            Table(end+1,:)   = [{'[0 , 360)','TOTAL'},num2cell(sum(count,1)),{sum(sum(count))}];
    Table(end+1,1:2) = {'No Direction', 'Wind Speed = 0'};  % Show Wind Speed = 0 on table.
    %% find max count
    colorBar1 = sum(count,2);
    foundIndex       = find(colorBar1 == max(colorBar1));
    colorSeq         = [mod(directions(foundIndex)-n,360),directions(foundIndex)+n];
    barIndex =  find(colorBar1>0);
    barX = [];
    barY = [];
for i = 1:length(barIndex)
    barX(i) = directions(i);
    barY(i) = colorBar1(i);
    end
    %bp = bar(barX,barY);
    %bp(1).LineWidth = 3;
    %bp(foundIndex).EdgeColor = 'red';

    Table{end,end}   = WindZeroFreqency;
```

<h2 id="3">3. 多项式拟合</h2>

可以自己更改n为不同的多项式

``` matlab
function [t,y1,a]=getfitLine(x,y,TapeLine)
    n=5;

    p = polyfit(x,y,n); 
    for i=1:n+1
    a(i)= p(i); 
    end
    t = linspace(x(1),x(length(x)),TapeLine);  
    if n==1;
    y1=a(1)*t+a(2);
    elseif n==2;
    y1=a(1)*t.^2+a(2)*t+a(3);
    elseif n==3;
    y1 = a(1)*t.^3 + a(2)*t.^2 + a(3)*t + a(4); 
    elseif n==4;
    y1 = a(1)*t.^4 + a(2)*t.^3 + a(3)*t.^2 + a(4)*t+a(5);
    elseif n==5
    y1= a(1)*t.^5+a(2)*t.^4+a(3)*t.^3+a(4)*t.^2+a(5)*t+a(6);
    end


    end

```

<h2 id="4">4. 不错的在图片中绘制箭头</h2>

``` matlab
    function hn=arrow3(p1,p2,s,w,h,ip,alpha,beta)
% ARROW3 (R13)
    %   ARROW3(P1,P2) draws lines from P1 to P2 with directional arrowheads.
    %   P1 and P2 are either nx2 or nx3 matrices.  Each row of P1 is an
    %   initial point, and each row of P2 is a terminal point.
    %
    %   ARROW3(P1,P2,S,W,H,IP,ALPHA,BETA) can be used to specify properties
    %   of the line, initial point marker, and arrowhead.  S is a character
    %   string made with one element from any or all of the following 3
    %   columns:
    %
    %     Color Switches      LineStyle            LineWidth
    %     ------------------  -------------------  --------------------
%     k  blacK (default)  -  solid (default)   0.5 points (default)
    %     y  Yellow           :  dotted            0   no lines
    %     m  Magenta          -. dashdot           /   LineWidthOrder
    %     c  Cyan             -- dashed
    %     r  Red              *  LineStyleOrder            _______ __  
    %     g  Green                                       ^        |    
    %     b  Blue                                       / \       |    
    %     w  White                        Arrowhead    /   \   Height  
    %     a  Asparagus                                /     \     |    
    %     d  Dark gray                               /       \    |    
    %     e  Evergreen                              /___   ___\ __|__  
    %     f  Firebrick                             |    | |    |       
    %     h  Hot pink                              |-- Width --|       
    %     i  Indigo                                |    | |    |       
    %     j  Jade                                       | |            
    %     l  Light gray                                 | |            
    %     n  Nutbrown                                   | |            
    %     p  Pear                                       | |            
    %     q  kumQuat                      Line       -->| |<--LineWidth
    %     s  Sky blue                                   | |            
    %     t  Tawny                                      | |            
    %     u  bUrgundy                                   | |            
    %     v  Violet                                     | |            
    %     z  aZure                                      | |            
    %     x  random                       Initial      /   \           
    %     o  colorOrder                   Point    -->(     )<--IP     
    %     |  magnitude                    Marker       \_ _/           
    %
    %     -------------------------------------------------------------
    %                          Color Equivalencies
    %     -------------------------------------------------------------
    %     ColorOrder     Arrow3         |     Simulink       Arrow3
    %     ----------     ----------     |     ----------     ----------
    %     Color1         Blue           |     LightBlue      aZure
    %     Color2         Evergreen      |     DarkGreen      Asparagus
    %     Color3         Red            |     Orange         kumQuat
    %     Color4         Sky blue       |     Gray           Light gray
    %     Color5         Violet         |
    %     Color6         Pear           |
    %     Color7         Dark gray      |
    %     -------------------------------------------------------------
    %
    %   The components of S may be specified in any order.  Invalid
    %   characters in S will be ignored and replaced by default settings.
    %
    %   Prefixing the color code with '_' produces a darker shade, e.g.
    %   '_t' is dark tawny; prefixing the color code with '^' produces a
    %   lighter shade, e.g. '^q' is light kumquat.  The relative brightness
    %   of light and dark color shades is controlled by the scalar parameter
    %   BETA.  Color code prefixes do not affect black (k), white (w), or
    %   the special color switches (xo|).
    %
    %   ColorOrder may be achieved in two fashions:  The user may either
    %   set the ColorOrder property (using RGB triples) or define the
    %   global variable ColorOrder (using a string of valid color codes).
    %   If the color switch is specified with 'o', and the global variable
    %   ColorOrder is a string of color codes (color switches less 'xo|',
            %   optionally prefixed with '_' or '^'), then the ColorOrder property
    %   will be set to the sequence of colors indicated by the ColorOrder
    %   variable.  The color sequence 'bersvpd' matches the default
    %   ColorOrder property.  If the color switch is specified with 'o', and
    %   the global variable ColorOrder is empty or invalid, then the current
    %   ColorOrder property will be used.  Note that the ColorOrder variable
    %   takes precedence over the ColorOrder property.
    %
    %   The magnitude color switch is used to visualize vector magnitudes
    %   in conjunction with a colorbar.  If the color switch is specified
    %   with '|', colors are linearly interpolated from the current ColorMap
    %   according to the length of the associated line.  This option sets
    %   CLim to [MinM,MaxM], where MinM and MaxM are the minimum and maximum
    %   magnitudes, respectively.
    %
    %   The current LineStyleOrder property will be used if LineStyle is
    %   specified with '*'.  MATLAB cycles through the line styles defined
    %   by the LineStyleOrder property only after using all colors defined
    %   by the ColorOrder property.  If however, the global variable
    %   LineWidthOrder is defined, and LineWidth is specified with '/',
    %   then each line will be drawn with sequential color, linestyle, and
    %   linewidth.
    %
    %   W (default = 1) is a vector of arrowhead widths; use W = 0 for no
    %   arrowheads.  H (default = 3W) is a vector of arrowhead heights.  If
    %   vector IP is neither empty nor negative, initial point markers will
    %   be plotted with diameter IP; for default diameter W, use IP = 0.
    %   The units of W, H and IP are 1/72 of the PlotBox diagonal.
    %
    %   ALPHA (default = 1) is a vector of FaceAlpha values ranging between
    %   0 (clear) and 1 (opaque).  FaceAlpha is a surface (arrowhead and
            %   initial point marker) property and does not affect lines.  FaceAlpha
    %   is not supported for 2D rendering.
    %
    %   BETA (default = 0.4) is a scalar that controls the relative
    %   brightness of light and dark color shades, ranging between 0 (no
            %   contrast) and 1 (maximum contrast).
    %
    %   Plotting lines with a single color, linestyle, and linewidth is
    %   faster than plotting lines with multiple colors and/or linestyles.
    %   Plotting lines with multiple linewidths is slower still.  ARROW3
    %   chooses renderers that produce the best screen images; exported
    %   or printed plots may benefit from different choices.
    %
    %   ARROW3(P1,P2,S,W,H,'cone',...) will plot cones with bases centered
    %   on P1 in the direction given by P2.  In this instance, P2 is
    %   interpreted as a direction vector instead of a terminal point.
    %   Neither initial point markers nor lines are plotted with the 'cone'
    %   option.
    %
    %   HN = ARROW3(P1,P2,...) returns a vector of handles to line and
    %   surface objects created by ARROW3.
    %
    %   ARROW3 COLORS will plot a table of named colors with default
    %   brightness.  ARROW3('colors',BETA) will plot a table of named
    %   colors with brightness BETA.
    %
    %   ARROW3 attempts to preserve the appearance of existing axes.  In
    %   particular, ARROW3 will not change XYZLim, View, or CameraViewAngle.
    %   ARROW3 does not, however, support stretch-to-fill scaling.  AXIS
    %   NORMAL will restore the current axis box to full size and remove any
    %   restrictions on the scaling of units, but will likely result in
    %   distorted arrowheads and initial point markers.  See
    %   (arrow3_messes_up_my_plots.html).
    %
    %   If a particular aspect ratio or variable limit is required, use
    %   DASPECT, PBASPECT, AXIS, or XYZLIM commands before calling ARROW3.
    %   Changing limits or aspect ratios after calling ARROW3 may alter the
    %   appearance of arrowheads and initial point markers.  ARROW3 sets
    %   XYZCLimMode to manual for all plots, sets DataAspectRatioMode to
    %   manual for linear plots, and sets PlotBoxAspectRatioMode to manual
    %   for log plots and 3D plots.  CameraViewAngleMode is also set to
    %   manual for 3D plots.
    %
    %   ARROW3 UPDATE will restore the appearance of arrowheads and
    %   initial point markers that have become corrupted by changes to
    %   limits or aspect ratios.  ARROW3('update',SF) will redraw initial
    %   point markers and arrowheads with scale factor SF.  If SF has one
%   element, SF scales W, H and IP.  If SF has two elements, SF(1)
    %   scales W and IP, and SF(2) scales H.  If SF has three elements,
    %   SF(1) scales W, SF(2) scales H, and SF(3) scales IP.  All sizes are
    %   relative to the current PlotBox diagonal.
    %
    %   ARROW3 UPDATE COLORS will update the magnitude coloring of
    %   arrowheads, initial point markers, and lines to conform to the
    %   current ColorMap.
    %
    %   HN = ARROW3('update',...) returns a vector of handles to updated
    %   objects.
    %
    %   EXAMPLES:
    %
    %     % 2D vectors
%     arrow3([0 0],[1 3])
    %     arrow3([0 0],[1 2],'-.e')
    %     arrow3([0 0],[10 10],'--x2',1)
    %     arrow3(zeros(10,2),50*rand(10,2),'x',1,3)
    %     arrow3(zeros(10,2),[10*rand(10,1),500*rand(10,1)],'u')
    %     arrow3(10*rand(10,2),50*rand(10,2),'x',1,[],1)
    %
    %     % 3D vectors
%     arrow3([0 0 0],[1 1 1])
    %     arrow3(zeros(20,3),50*rand(20,3),'--x1.5',2)
    %     arrow3(zeros(100,3),50*rand(100,3),'x',1,3)
    %     arrow3(zeros(10,3),[10*rand(10,1),500*rand(10,1),50*rand(10,1)],'a')
    %     arrow3(10*rand(10,3),50*rand(10,3),'x',[],[],0)
    %
    %     % 3D animation
    %     t=(0:pi/40:8*pi)'; u=cos(t); v=sin(t);
    %     plot3(20*t,u,v)
    %     axis([0,600,-1.5,1.5,-1.5,1.5])
%     grid on, view(35,25)
    %     hold on
%     pbaspect([1.8,1.4,1])
    %     arrow3(zeros(3),diag([500,1.5,1.5]),'l',0.7,[],0)
    %     p=[20*t,u,v]; inc=4:1:length(t);
    %     p2=p(inc,:); p1=p(inc-1,:);
    %     hn=arrow3(p1(1,:),p2(1,:),'0_b',0.7);
    %     for i=2:1:length(p1)
%       delete(hn)
    %       hn=arrow3(p1(i,:),p2(i,:),'0_b',0.7);
%       pause(0.01)
    %     end
    %     hold off
    %
    %     % Cone plot
    %     t=(pi/8:pi/8:2*pi)'; p1=[cos(t) sin(t) t]; p2=repmat([0 0 1],16,1);
    %     arrow3(p1,p2,'x',2,4,'cone'), hold on
    %     plot3(p1(:,1),p1(:,2),p1(:,3)), hold off
    %     pause % change cone size
    %     arrow3('update',[1,2])
    %
    %     % Just for fun
    %     arrow3(zeros(100,3),50*rand(100,3),'x',8,4,[],0.95)
    %     light('position',[-10 -10 -10],'style','local')
    %     light('position',[60,60,60]), lighting gouraud
    %
    %     % ColorOrder variable, color code prefixes, and Beta
    %     global ColorOrder, ColorOrder='^ui^e_hq^v';
    %     theta=[0:pi/22:pi/2]';
    %     arrow3(zeros(12,2),[cos(theta),sin(theta)],'1.5o',1.5,[],[],[],0.5)
    %
    %     % ColorOrder property, LineStyleOrder, and LineWidthOrder
    %     global ColorOrder, ColorOrder=[];
    %     set(gca,'ColorOrder',[1,0,0;0,0,1;0.25,0.75,0.25;0,0,0])
    %     set(gca,'LineStyleOrder',{'-','--','-.',':'})
    %     global LineWidthOrder, LineWidthOrder=[1,2,4,8];
    %     w=[1,2,3,4]; h=[4,6,4,2];
    %     arrow3(zeros(4,2),[10*rand(4,1),500*rand(4,1)],'o*/',w,h,0)
    %
    %     % Magnitude coloring
    %     colormap spring
    %     arrow3(20*randn(20,3),50*randn(20,3),'|',[],[],0)
    %     set(gca,'color',0.7*[1,1,1])
    %     set(gcf,'color',0.5*[1,1,1]), grid on, colorbar
    %     pause % change the ColorMap and update colors
    %     colormap hot
    %     arrow3('update','colors')
    %
    %     % LogLog plot
    %     set(gca,'xscale','log','yscale','log');
    %     axis([1e2,1e8,1e-2,1e-1]); hold on
    %     p1=repmat([1e3,2e-2],15,1);
    %     q1=[1e7,1e6,1e5,1e4,1e3,1e7,1e7,1e7,1e7,1e7,1e7,1e6,1e5,1e4,1e3];
    %     q2=1e-2*[9,9,9,9,9,7,5,4,3,2,1,1,1,1,1]; p2=[q1',q2'];
    %     global ColorOrder, ColorOrder=[];
    %     set(gca,'ColorOrder',rand(15,3))
    %     arrow3(p1,p2,'o'), grid on, hold off
    %
    %     % SemiLogX plot
    %     set(gca,'xscale','log','yscale','linear');
    %     axis([1e2,1e8,1e-2,1e-1]); hold on
    %     p1=repmat([1e3,0.05],15,1);
    %     q1=[1e7,1e6,1e5,1e4,1e3,1e7,1e7,1e7,1e7,1e7,1e7,1e6,1e5,1e4,1e3];
    %     q2=1e-2*[9,9,9,9,9,7,5,4,3,2,1,1,1,1,1]; p2=[q1',q2'];
    %     arrow3(p1,p2,'x'), grid on, hold off
    %
    %     % SemiLogY plot
    %     set(gca,'xscale','linear','yscale','log');
    %     axis([2,8,1e-2,1e-1]); hold on
    %     p1=repmat([3,2e-2],17,1);
    %     q1=[7,6,5,4,3,7,7,7,7,7,7,7,7,6,5,4,3];
    %     q2=1e-2*[9,9,9,9,9,8,7,6,5,4,3,2,1,1,1,1,1]; p2=[q1',q2'];
    %     set(gca,'LineStyleOrder',{'-','--','-.',':'})
    %     arrow3(p1,p2,'*',1,[],0), grid on, hold off
    %
    %     % Color tables
    %     arrow3('colors')           % default color table
    %     arrow3('colors',0.3)       % low contrast color table
    %     arrow3('colors',0.5)       % high contrast color table
    %
    %     % Update initial point markers and arrowheads
    %     % relative to the current PlotBox diagonal
    %     arrow3('update')           % redraw same size
    %     arrow3('update',2)         % redraw double size
    %     arrow3('update',0.5)       % redraw half size
    %     arrow3('update',[0.5,2,1]) % redraw W half size,
    %                                %        H double size, and
    %                                %        IP same size
    %
    %     See also (arrow3_examples.html), (arrow3_messes_up_my_plots.html).

    %   Copyright(c)2002-2011 Version 5.14
%     Tom Davis (tdavis@metzgerwillard.com)
    %     Jeff Chang

    %   Revision History:
    %
    %     07/27/11 - Added animation example. (TD)
    %     05/13/09 - Corrected spelling errors (TD)
    %     03/16/08 - Updated contact information (TD)
%     10/23/07 - Corrected zero magnitude exclusion (TD)
    %     09/08/07 - Added cone plot option; removed adaptive grid
    %                spacing; corrected scale factor; removed "nearly"
    %                tight limits (TD)
%     07/24/07 - Ignore zero-magnitude input (TD)
    %     07/08/07 - Modified named colors to match named Simulink
    %                colors; added light and dark shades for basic
%                colors (ymcrgb) (TD)
    %     07/01/07 - Modified named colors to match default ColorOrder
    %                colors (TD)
    %     06/24/07 - Error checking for empty P1, P2 (TD)
    %     06/17/07 - Trim colors,W,H,IP,ALPHA to LENGTH(P1) (TD)
    %     05/27/07 - Magnitude coloring and documentation revision (TD)
%     03/10/07 - Improved code metrics (TD)
    %     02/21/07 - Preserve existing axis appearance;
    %                use relative sizes for W, H, and IP;
    %                removed version checking; minor bug fixes (TD) 
    %     01/09/04 - Replaced calls to LINSPACE, INTERP1, and
%                COLORMAP (TD)
    %     12/17/03 - Semilog examples, CAXIS support, magnitude
    %                coloring, and color updating; use CData instead
    %                of FaceColor; minor bug fixes (TD)
    %     07/17/03 - Changed 2D rendering from OpenGL to ZBuffer;
%                defined HN for COLORS and UPDATE options (TD)
    %     02/27/03 - Replaced calls to RANDPERM, VIEW, REPMAT, SPHERE,
    %                and CYLINDER; added ZBuffer for log plots, RESET
%                for CLA and CLF, and ABS for W and H (TD)
    %     02/01/03 - Added UPDATE scale factor and MATLAB version
    %                checking, replaced call to CROSS (TD)
%     12/26/02 - Added UserData and UPDATE option (TD)
    %     11/16/02 - Added more named colors, color code prefix,
    %                global ColorOrder, ALPHA , and BETA (TD)
    %     10/12/02 - Added global LineWidthOrder,
    %                vectorized W, H and IP (TD)
    %     10/05/02 - Changed CLF to CLA for subplot support,
    %                added ColorOrder and LineStyleOrder support (TD)
    %     04/27/02 - Minor log plot revisions (TD)
%     03/26/02 - Added log plot support (TD)
    %     03/24/02 - Adaptive grid spacing control to trade off
%                appearance vs. speed based on size of matrix (JC)
    %     03/16/02 - Added "axis tight" for improved appearance (JC)
    %     03/12/02 - Added initial point marker (TD)
%     03/03/02 - Added aspect ratio support (TD)
    %     03/02/02 - Enhanced program's user friendliness (JC)
    %                (lump Color, LineStyle, and LineWidth together)
%     03/01/02 - Replaced call to ROTATE (TD)
    %     02/28/02 - Modified line plotting,
    %                added linewidth and linestyle (TD)
    %     02/27/02 - Minor enhancements on 3D appearance (JC)
    %     02/26/02 - Minor enhancements for speed (TD&JC)
%     02/26/02 - Optimize PLOT3 and SURF for speed (TD)
    %     02/25/02 - Return handler, error handling, color effect,
    %                generalize for 2D/3D vectors (JC)
    %     02/24/02 - Optimize PLOT3 and SURF for speed (TD)
%     02/23/02 - First release (JC&TD)

    %-------------------------------------------------------------------------
    % Error Checking
    global LineWidthOrder ColorOrder
    if nargin<8 || isempty(beta), beta=0.4; end
    beta=abs(beta(1)); if nargout, hn=[]; end
    if strcmpi(p1,'colors')                            % plot color table
    if nargin>1, beta=abs(p2(1)); end
    LocalColorTable(1,beta); return
    end
    fig=gcf; ax=gca;
    if strcmpi(p1,'update'), ud=get(ax,'UserData');    % update
    LocalLogCheck(ax);
    if size(ud,2)<13, error('Invalid UserData'), end
    set(ax,'UserData',[]); sf=[1,1,1]; flag=0;
    if nargin>1
    if strcmpi(p2,'colors'), flag=1;               % update colors
    elseif ~isempty(p2)                            % update surfaces
    sf=p2(1)*sf; n=length(p2(:));
    if n>1, sf(2)=p2(2); if n>2, sf(3)=p2(3); end, end
    end
    end
    H=LocalUpdate(fig,ax,ud,sf,flag); if nargout, hn=H; end, return
    end
    InputError=['Invalid input, type HELP ',upper(mfilename),...
    ' for usage examples'];
    if nargin<2, error(InputError), end
    [r1,c1]=size(p1); [r2,c2]=size(p2);
    if c1<2 || c1>3 || r1*r2==0, error(InputError), end
    if r1~=r2, error('P1 and P2 must have same number of rows'), end
    if c1~=c2, error('P1 and P2 must have same number of columns'), end
    p=sum(abs(p2-p1),2)~=0; cone=0;
    if nargin>5 && ~isempty(ip) && strcmpi(ip,'cone')  % cone plot
    cone=1; p=sum(p2,2)~=0;
    if ~any(p), error('P2 cannot equal 0'), end
    set(ax,'tag','Arrow3ConePlot');
    elseif ~any(p), error('P1 cannot equal P2')
    end
if ~all(p)
    warning('Arrow3:ZeroMagnitude','Zero magnitude ignored')
    p1=p1(p,:); p2=p2(p,:); [r1,c1]=size(p1);
    end
    n=r1; Zeros=zeros(n,1);
    if c1==2, p1=[p1,Zeros]; p2=[p2,Zeros];
    elseif ~any([p1(:,3);p2(:,3)]), c1=2; end
    L=get(ax,'LineStyleOrder'); C=get(ax,'ColorOrder');
    ST=get(ax,'DefaultSurfaceTag'); LT=get(ax,'DefaultLineTag');
    EC=get(ax,'DefaultSurfaceEdgeColor');
    if strcmp(get(ax,'nextplot'),'add') && strcmp(get(fig,'nextplot'),'add')
    Xr=get(ax,'xlim'); Yr=get(ax,'ylim'); Zr=get(ax,'zlim');
    [xs,ys,xys]=LocalLogCheck(ax); restore=1;
    if xys, mode='auto';
    if any([p1(:,3);p2(:,3)]), error('3D log plot not supported'), end
    if (xs && ~all([p1(:,1);p2(:,1)]>0)) || ...
    (ys && ~all([p1(:,2);p2(:,2)]>0))
    error('Nonpositive log data not supported')
    end
    else mode='manual';
    if strcmp(get(ax,'WarpToFill'),'on')
    warning('Arrow3:WarpToFill',['Stretch-to-fill scaling not ',...
            'supported;\nuse DASPECT or PBASPECT before calling ARROW3.']);
    end
    end
    set(ax,'XLimMode',mode,'YLimMode',mode,'ZLimMode',mode,...
            'CLimMode','manual');
    else restore=0; cla reset; xys=0; set(fig,'nextplot','add');
    if c1==2, azel=[0,90]; else azel=[-37.5,30]; end
    set(ax,'UserData',[],'nextplot','add','View',azel);
    end

    %-------------------------------------------------------------------------
    % Style Control
    [vc,cn]=LocalColorTable(0); prefix=''; OneColor=0;
    if nargin<3, [c,ls,lw]=LocalValidateCLSW;% default color, linestyle/width
    else 
    [c,ls,lw]=LocalValidateCLSW(s);
    if length(c)>1, if sum('_^'==c(1)), prefix=c(1); end, c=c(2); end
    if c=='x'                              % random named color (less white)
    [ignore,i]=sort(rand(1,23)); c=cn(i,:);        %#ok
    elseif c=='o'                                    % ColorOrder
if length(ColorOrder)
    [c,failed]=LocalColorMap(lower(ColorOrder),vc,cn,beta);
    if failed, ColorOrderWarning=['Invalid ColorOrder ',...
    'variable, current ColorOrder property will be used'];
    warning('Arrow3:ColorOrder',ColorOrderWarning)
    else C=c;
    end
    end, c=C;
    elseif c=='|', map=get(fig,'colormap');          % magnitude coloring
    M=(p1-p2); M=sqrt(sum(M.*M,2)); minM=min(M); maxM=max(M);
    if maxM-minM<1, minM=0; end
    set(ax,'clim',[minM,maxM]); c=LocalInterp(minM,maxM,map,M);
    elseif ~sum(vc==c), c='k'; ColorWarning=['Invalid color switch, ',...
    'default color (black) will be used'];
    warning('Arrow3:Color',ColorWarning)
    end
    end
    if length(c)==1                                    % single color
    c=LocalColorMap([prefix,c],vc,cn,beta); OneColor=1;
    end
    set(ax,'ColorOrder',c); c=LocalRepmat(c,[ceil(n/size(c,1)),1]);
    if ls~='*', set(ax,'LineStyleOrder',ls); end       % LineStyleOrder
    if lw=='/'                                         % LineWidthOrder
if length(LineWidthOrder)
    lw=LocalRepmat(LineWidthOrder(:),[ceil(n/length(LineWidthOrder)),1]);
    else lw=0.5; LineWidthOrderWarning=['Undefined LineWidthOrder, ',...
    'default width (0.5) will be used'];
    warning('Arrow3:LineWidthOrder',LineWidthOrderWarning)
    end
    end
    if nargin<4 || isempty(w), w=1; end                % width
    w=LocalRepmat(abs(w(:)),[ceil(n/length(w)),1]);
    if nargin<5 || isempty(h), h=3*w; end              % height
    h=LocalRepmat(abs(h(:)),[ceil(n/length(h)),1]);
    if nargin>5 && ~isempty(ip) && ~cone               % ip
    ip=LocalRepmat(ip(:),[ceil(n/length(ip)),1]);
    i=find(ip==0); ip(i)=w(i);
    else ip=-ones(n,1);
    end
    if nargin<7 || isempty(alpha), alpha=1; end
    a=LocalRepmat(alpha(:),[ceil(n/length(alpha)),1]); % FaceAlpha

    %-------------------------------------------------------------------------
    % Log Plot
    if xys
    units=get(ax,'units'); set(ax,'units','points');
    pos=get(ax,'position'); set(ax,'units',units);
    if strcmp(get(ax,'PlotBoxAspectRatioMode'),'auto')
    set(ax,'PlotBoxAspectRatio',[pos(3),pos(4),1]);
    end
    par=get(ax,'PlotBoxAspectRatio');
    set(ax,'DataAspectRatio',[par(2),par(1),par(3)]);
    % map coordinates onto unit square
    q=[p1;p2]; xr=Xr; yr=Yr;
    if xs, xr=log10(xr); q(:,1)=log10(q(:,1)); end
    if ys, yr=log10(yr); q(:,2)=log10(q(:,2)); end
    q=q-LocalRepmat([xr(1),yr(1),0],[2*n,1]);
    dx=xr(2)-xr(1); dy=yr(2)-yr(1);
    q=q*diag([1/dx,1/dy,1]);
    q1=q(1:n,:); q2=q(n+1:end,:);
    else xs=0; ys=0; dx=0; dy=0; xr=0; yr=0;
    end

    %-------------------------------------------------------------------------
    % Line
    if ~cone
    set(ax,'DefaultLineTag','arrow3');
    if length(lw)==1
    if lw>0
    if OneColor && ls(end)~='*' && n>1 % single color, linestyle/width
    P=zeros(3*n,3); i=1:n;
    P(3*i-2,:)=p1(i,:); P(3*i-1,:)=p2(i,:); P(3*i,1)=NaN;
    H1=plot3(P(:,1),P(:,2),P(:,3),'LineWidth',lw);
    else                               % single linewidth
    H1=plot3([p1(:,1),p2(:,1)]',[p1(:,2),p2(:,2)]',...
            [p1(:,3),p2(:,3)]','LineWidth',lw);
    end
    else H1=[];
    end
    else                                   % use LineWidthOrder
    ls=LocalRepmat(cellstr(L),[ceil(n/size(L,1)),1]);
    H1=Zeros;
    for i=1:n
    H1(i)=plot3([p1(i,1),p2(i,1)],[p1(i,2),p2(i,2)],...
            [p1(i,3),p2(i,3)],ls{i},'Color',c(i,:),'LineWidth',lw(i));
    end
    end
    else                                     % cone plot
    P=zeros(3*n,3); i=1:n;
    P(3*i-2,:)=p1(i,:); P(3*i-1,:)=p1(i,:); P(3*i,1)=NaN;
    H1=plot3(P(:,1),P(:,2),P(:,3));
    end

    %-------------------------------------------------------------------------
    % Scale
    if ~restore, axis tight, end
    ar=get(ax,'DataAspectRatio'); ar=sqrt(3)*ar/norm(ar);
    set(ax,'DataAspectRatioMode','manual');
    if xys, sf=1;
    else xr=get(ax,'xlim'); yr=get(ax,'ylim'); zr=get(ax,'zlim');
    sf=norm(diff([xr;yr;zr],1,2)./ar')/72;
    end

    %-------------------------------------------------------------------------
    % UserData
    c=c(1:n,:); w=w(1:n); h=h(1:n); ip=ip(1:n); a=a(1:n);
    set(ax,'UserData',[get(ax,'UserData');p1,p2,c,w,h,ip,a]);

    %-------------------------------------------------------------------------
    % Arrowhead
    whip=sf*[w,h,ip];
    if xys, whip=whip*sqrt(2)/72; p1=q1; p2=q2; end
    w=whip(:,1); h=whip(:,2); ip=whip(:,3);
    if cone                                            % cone plot
    delete(H1), H1=[];
    p2=p2./LocalRepmat(sqrt(sum(p2.*p2,2)),[1,3]);
    p2=p1+p2.*LocalRepmat(ar,[n,1]).*LocalRepmat(h,[1,3]);
    end
    W=(p1-p2)./LocalRepmat(ar,[n,1]);
    W=W./LocalRepmat(sqrt(sum(W.*W,2)),[1,3]);         % new z direction
    U=[-W(:,2),W(:,1),Zeros];
    N=sqrt(sum(U.*U,2)); i=find(N<eps); j=length(i);
    U(i,:)=LocalRepmat([1,0,0],[j,1]); N(i)=ones(j,1);
    U=U./LocalRepmat(N,[1,3]);                         % new x direction
    V=[W(:,2).*U(:,3)-W(:,3).*U(:,2),...               % new y direction
    W(:,3).*U(:,1)-W(:,1).*U(:,3),...
    W(:,1).*U(:,2)-W(:,2).*U(:,1)];

    m=20;                               % surface grid spacing
    set(ax,'DefaultSurfaceTag','arrow3','DefaultSurfaceEdgeColor','none');
    r=[0;1]; theta=(0:m)/m*2*pi; Ones=ones(1,m+1);
    x=r*cos(theta); y=r*sin(theta); z=r*Ones;
    G=surface(x/2,y/2,z); dar=diag(ar);
    X=get(G,'XData'); Y=get(G,'YData'); Z=get(G,'ZData');
    H2=Zeros; [j,k]=size(X);
    for i=1:n                           % translate, rotate, and scale
    H2(i)=copyobj(G,ax);
    xyz=[w(i)*X(:),w(i)*Y(:),h(i)*Z(:)]*[U(i,:);V(i,:);W(i,:)]*dar;
    x=reshape(xyz(:,1),j,k)+p2(i,1);
    y=reshape(xyz(:,2),j,k)+p2(i,2);
    z=reshape(xyz(:,3),j,k)+p2(i,3);
    LocalSetSurface(xys,xs,ys,dx,dy,xr,yr,...
            x,y,z,a(i),c(i,:),H2(i),2,m+1);
    end
    delete(G);

    %-------------------------------------------------------------------------
    % Initial Point Marker
if any(ip>0)
    theta=(-m:2:m)/m*pi; phi=(-m:2:m)'/m*pi/2; cosphi=cos(phi);
    x=cosphi*cos(theta); y=cosphi*sin(theta); z=sin(phi)*Ones;
    G=surface(x*ar(1)/2,y*ar(2)/2,z*ar(3)/2);
    X=get(G,'XData'); Y=get(G,'YData'); Z=get(G,'ZData');
    H3=zeros(n,1);
    for i=1:n                                        % translate
    if ip(i)>0
    H3(i)=copyobj(G,ax);
    x=p1(i,1)+X*ip(i); y=p1(i,2)+Y*ip(i); z=p1(i,3)+Z*ip(i);
    LocalSetSurface(xys,xs,ys,dx,dy,xr,yr,...
            x,y,z,a(i),c(i,:),H3(i),m+1,m+1);
    end
    end, delete(G);
    else H3=[];
    end

    %-------------------------------------------------------------------------
    % Finish
    if restore, xr=Xr; yr=Yr; zr=Zr;
    if xys, set(ax,'DataAspectRatioMode','auto'); end
    else
    axis tight
    xr=get(ax,'xlim'); yr=get(ax,'ylim'); zr=get(ax,'zlim');
    set(ax,'nextplot','replace');
    end
    azel=get(ax,'view');
    if abs(azel(2))==90, renderer='ZBuffer'; else renderer='OpenGL'; c1=3; end
    set(fig,'Renderer',renderer);
    set(ax,'LineStyleOrder',L,'ColorOrder',C,'DefaultLineTag',LT,...
            'DefaultSurfaceTag',ST,'DefaultSurfaceEdgeColor',EC,...
            'xlim',xr,'ylim',yr,'zlim',zr,'clim',get(ax,'CLim'));
    if c1==3, set(ax,'CameraViewAngle',get(ax,'CameraViewAngle'),...
            'PlotBoxAspectRatio',get(ax,'PlotBoxAspectRatio'));
    end
    if nargout, hn=[H1(:);H2(:);H3(:)]; end

    %-------------------------------------------------------------------------
    % Local Functions
    %-------------------------------------------------------------------------
    % Update
function H=LocalUpdate(fig,ax,ud,sf,flag)
    global ColorOrder
    p1=ud(:,1:3); p2=ud(:,4:6); c=ud(:,7:9); a=ud(:,13);
    w=sf(1)*ud(:,10); h=sf(2)*ud(:,11); ip=sf(3)*ud(:,12);
    H=get(ax,'children'); tag=get(H,'tag'); type=get(H,'type');
    delete(H(strcmp(tag,'arrow3') & strcmp(type,'surface')));
    set(fig,'nextplot','add'); set(ax,'nextplot','add'); H1=[];
    if flag, map=get(fig,'colormap');                  % update colors
    M=(p1-p2); M=sqrt(sum(M.*M,2)); minM=min(M); maxM=max(M);
    H1=H(strcmp(tag,'arrow3') & strcmp(type,'line'));
    MagnitudeWarning=['Cannot perform magnitude coloring on lines ',...
    'that\nwere drawn with a single color, linestyle, and linewidth'];
    if length(H1)>1
    for i=1:length(H1)                             % update line colors
    x=get(H1(i),'xdata'); y=get(H1(i),'ydata'); z=get(H1(i),'zdata');
    if length(x)>2                               % multiple lines
    warning('Arrow3:Magnitude',MagnitudeWarning), continue
    end
    m=sqrt((x(1)-x(2))^2+(y(1)-y(2))^2+(z(1)-z(2))^2);
    c=LocalInterp(minM,maxM,map,m); set(H1(i),'color',c);
    end
    elseif length(H1)==1
    warning('Arrow3:Magnitude',MagnitudeWarning)
    end
    c=LocalInterp(minM,maxM,map,M);
    end
    set(ax,'ColorOrder',c);                            % update surfaces
    ColorOrder=[];
    if strcmp(get(ax,'tag'),'Arrow3ConePlot')
    H=arrow3(p1,p2,'o' ,w,h,'cone',a);            % update cones
    else H=arrow3(p1,p2,'o0',w,h,    ip,a);
    end, H=[H1(:);H(:)];
    set(ax,'nextplot','replace');

    %-------------------------------------------------------------------------
    % SetSurface
function LocalSetSurface(xys,xs,ys,dx,dy,xr,yr,x,y,z,a,c,H,n,m)
    if xys
    x=x*dx+xr(1); y=y*dy+yr(1);
    if xs, x=10.^x; end
    if ys, y=10.^y; end
    end
    cd=zeros(n,m,3); cd(:,:,1)=c(1); cd(:,:,2)=c(2); cd(:,:,3)=c(3);
    set(H,'XData',x,'YData',y,'ZData',z,'CData',cd,'FaceAlpha',a);

    %-------------------------------------------------------------------------
    % ColorTable
function [vc,cn]=LocalColorTable(n,beta)
    vc='kymcrgbadefhijlnpqstuvzw';                     % valid color codes
    %                k               y               m               c
    cn=[0.00,0.00,0.00; 1.00,1.00,0.00; 1.00,0.00,1.00; 0.00,1.00,1.00;
    %                r               g               b               a
    1.00,0.00,0.00; 0.00,1.00,0.00; 0.00,0.00,1.00; 0.42,0.59,0.24;
    %                d               e               f               h
    0.25,0.25,0.25; 0.00,0.50,0.00; 0.70,0.13,0.13; 1.00,0.41,0.71;
    %                i               j               l               n
    0.29,0.00,0.51; 0.00,0.66,0.42; 0.50,0.50,0.50; 0.50,0.20,0.00;
    %                p               q               s               t
    0.75,0.75,0.00; 1.00,0.50,0.00; 0.00,0.75,0.75; 0.80,0.34,0.00;
    %                u               v               z               w
    0.50,0.00,0.13; 0.75,0.00,0.75; 0.38,0.74,0.99; 1.00,1.00,1.00];

% Named Simulink Colors (zaql)
    % LightBlue = 0.38  0.74  0.99 = aZure
    % DarkGreen = 0.42  0.59  0.24 = Asparagus
    % Orange    = 1.00  0.50  0.00 = kumQuat
    % Gray      = 0.50  0.50  0.50 = Light gray
    %
% Default ColorOrder Property Colors (bersvpd)
    % Color1    = 0.00  0.00  1.00 = Blue
    % Color2    = 0.00  0.50  0.00 = Evergreen
    % Color3    = 1.00  0.00  0.00 = Red
    % Color4    = 0.00  0.75  0.75 = Sky blue
    % Color5    = 0.75  0.00  0.75 = Violet
    % Color6    = 0.75  0.75  0.00 = Pear
    % Color7    = 0.25  0.25  0.25 = Dark gray

    if n, clf reset                                    % plot color table
    name={'blacK','Yellow','Magenta','Cyan',...
        'Red','Green','Blue','Asparagus',...
            'Dark gray','Evergreen','Firebrick','Hot pink',...
            'Indigo','Jade','Light gray','Nutbrown',...
            'Pear','kumQuat','Sky blue','Tawny',...
            'bUrgundy','Violet','aZure','White'};
c=['yptn';'gjae';'czsb';'hmvi';'qrfu';'wldk'];
set(gcf,'DefaultAxesXTick',[],'DefaultAxesYTick',[],...
        'DefaultAxesXTickLabel',[],'DefaultAxesYTickLabel',[],...
        'DefaultAxesXLim',[0,0.75],'DefaultAxesYLim',[0,0.75],...
        'DefaultRectangleEdgeColor','none');
for i=1:24, subplot(4,6,i); box on
j=find(vc==c(i)); title(name{j});
dark=LocalBrighten(cn(j,:),-beta);
light=LocalBrighten(cn(j,:),beta);
rectangle('Position',[0,0.00,0.75,0.25],'FaceColor',dark);
rectangle('Position',[0,0.25,0.75,0.25],'FaceColor',cn(j,:));
rectangle('Position',[0,0.50,0.75,0.25],'FaceColor',light);
rectangle('Position',[0,0.00,0.75,0.75],'EdgeColor','k');
if rem(i,6)==1
set(gca,'YTickLabel',{'dark','normal','light'},...
        'YTick',[0.125,0.375,0.625]);
if i==19
text(0,-0.25,['{\bf\itARROW3}  Named Color Table  ',...
        '( \beta = ',num2str(beta),' )']);
end
end
end
end

%-------------------------------------------------------------------------
    % ColorMap
function [C,failed]=LocalColorMap(c,vc,cn,beta)
    n=length(c); failed=0; C=zeros(n,3); i=1; j=1;
    while 1
    if ~sum([vc,'_^']==c(i)), failed=1; break, end
    if sum('_^'==c(i))
    if i+1>n, failed=1; break, end
    if ~sum(vc==c(i+1)), failed=1; break, end
    cc=cn(vc==c(i+1),:); gamma=beta;
    if c(i)=='_', gamma=-beta; end
    C(j,:)=LocalBrighten(cc,gamma); i=i+2;
    else C(j,:)=cn(vc==c(i),:); i=i+1;
    end
    if i>n, break, end, j=j+1;
    end
    if n>j, C(j+1:n,:)=[]; end

    %-------------------------------------------------------------------------
    % Brighten
function C=LocalBrighten(c,beta)
    if sum([c==0,c==1])==3 && sum(c==0)<3 && sum(c==1)<3
    if beta<0
    C=(1+beta)*c;
    else
    C=c;  C(C==0)=beta;
    end
    else
    C=c.^((1-min(1-sqrt(eps),abs(beta)))^sign(beta));
    end

    %-------------------------------------------------------------------------
    % Repmat
function B=LocalRepmat(A,siz)
    if length(A)==1, B(prod(siz))=A; B(:)=A; B=reshape(B,siz);
    else [m,n]=size(A); mind=(1:m)'; nind=(1:n)';
    mind=mind(:,ones(1,siz(1))); nind=nind(:,ones(1,siz(2)));
    B=A(mind,nind);
    end

    %-------------------------------------------------------------------------
    % Interp
function v=LocalInterp(xmin,xmax,y,u)
    [m,n]=size(y); h=(xmax-xmin)/(m-1); p=length(u); v=zeros(p,n);
    k=min(max(1+floor((u-xmin)/h),1),m-1); s=(u-xmin)/h-k+1;
    for j=1:n, v(:,j)=y(k,j)+s.*(y(k+1,j)-y(k,j)); end
    v(v<0)=0; v(v>1)=1;

    %-------------------------------------------------------------------------
    % Check for supported log scales
function [xs,ys,xys]=LocalLogCheck(ax)
    xs=strcmp(get(ax,'xscale'),'log');
    ys=strcmp(get(ax,'yscale'),'log');
    zs=strcmp(get(ax,'zscale'),'log');
    if zs, error('Z log scale not supported'), end
    xys=xs+ys;
    if xys, azel=get(ax,'view');
    if abs(azel(2))~=90, error('3D log plot not supported'), end
    end

    %-------------------------------------------------------------------------
    % Generate valid value for color, linestyle and linewidth
function [c,ls,lw]=LocalValidateCLSW(s)
    if nargin<1, c='k'; ls='-'; lw=0.5;
    else
    % identify linestyle
    if findstr(s,'--'), ls='--'; s=strrep(s,'--','');
    elseif findstr(s,'-.'), ls='-.'; s=strrep(s,'-.','');
    elseif findstr(s,'-'), ls='-'; s=strrep(s,'-','');
    elseif findstr(s,':'), ls=':'; s=strrep(s,':','');
    elseif findstr(s,'*'), ls='*'; s=strrep(s,'*','');
    else ls='-';
    end

    % identify linewidth
    tmp=double(s);
    tmp=find(tmp>45 & tmp<58);
if length(tmp)
    if any(s(tmp)=='/'), lw='/'; else lw=str2double(s(tmp)); end
    s(tmp)='';
    else lw=0.5;
    end

    % identify color
    if length(s), s=lower(s);
    if length(s)>1, c=s(1:2);
    else c=s(1); end
    else c='k';
    end
    end

```

<h2 id="5">5. 误差带</h2>
    有时候可能输入平均值，和上下线误差，该程序比内置的errorbar更好用一些

```matlab
function errorbare(sty,x,y,xbar,ybar,symbol)

    % ERRORBARE Enhanced Errorbar Function.
%   ERRORBARE(STY,X,Y,Xbar,Ybar,symbol) 
    %   It can draw errorbar along X/Y/Dual axis 
    %   in normal,semilog,loglog coordinate system,
    %   and adjust length of top line automatically,
    %   can also control dotstyle and color in the same way with errorbar.
    %
    %   If the lower and upper error range of x/y is different, they should be
    %   input as [lower,upper] if x/y is a column vector; 
    %   for a row vector, they should be [lower;uper].
    %
    %   parameter STY include 12 types: 
    %   v,h,d,vlogx,hlogx,dlogx,vlogy,hlogy,
    %       dlogy,vlogd,hlogd,dlogd 
    %   where
    %   v stands for vertical errorbar，
    %   h draws horizontal errorbar，
    %   d means dual direction,
    %   logx corresponding to semilogx，can use preffix v/h/d
    %   logy corresponding to semilogy，can use preffix v/h/d
    %   logd corresponding to loglog，can use preffix v/h/d

    %   误差棒函数增强版
%   ERRORBARE(STY,X,Y,Xbar,Ybar,symbol) 
    %   可在各个坐标系中沿X轴，Y轴方向，或者两轴方向绘制误差棒，
    %   能够根据所选坐标类型调整端点线长。
    %   增加对误差棒的线型控制，用法与原errorbar函数中相同
    %
    %   若上下限范围不同，X为列向量时应按照
    %   [下限,上限] 的格式输入，若为行向量则为 [下限;上限]
    %
    %   STY 参数包括 v,h,d,vlogx,hlogx,dlogx,vlogy,hlogy,
    %	dlogy,vlogd,hlogd,dlogd 共12种
    %   v 表示误差棒垂直，
    %   h 表示误差棒水平，
    %   d (dual) 显示双轴误差，
    %   logx 对应 semilogx，前缀 v,h,d 意义同上
    %   logy 对应 semilogy，前缀 v,h,d 意义同上
    %   logd 对应 loglog，前缀 v,h,d 意义同上

    %   For example,
    %	x = 1:10;
%	y = sin(x)+2;
%	e = std(y)*ones(size(x));
%	errorbare(x,y,e)	% use function "errorbar" directly
%	errorbare(x,y,e,'or')
%	errorbare('v',x,y,e)	% "e" is error of "y"
%	errorbare('v',x,y,[e;2*e])  % try different error limits
%	errorbare('hlogx',x,y,e)    % "e" is error of "x" here，
%	errorbare('d',x,y,e,e)
%	errorbare('d',x,y,e,e,'or')
%	errorbare('dlogd',x,y,e,e)
%
%   by Henry Sting  Email: henrysting@hotmail.com
%   $Revision: 1.2 $  $Date: 2010-2-8 $

lx=[];ux=[];ly=[];uy=[]; % 误差棒上下限
xl=[];xr=[];yl=[];yr=[]; % 端点短线左右限

if ~isstr(sty)
    if nargin == 3
errorbar(sty,x,y)
    return
    elseif nargin == 4
errorbar(sty,x,y,xbar)
    return
    elseif nargin > 4
    error('Please assign adopted coordinate system with symbol parameters.')
    end
    elseif isstr(sty)
if size(x)~=size(y)
    error('Coordinate array should be equal.')
    end
    if nargin == 4
    symbol='ob';
if length(x)~=length(xbar)
    error('Format of Xbar is illegal.')
    end
    if sty(1) == 'v'
    ybar=xbar;xbar=[];
    elseif sty(1) == 'h'
    ybar=[];
    elseif sty(1) == 'd'
    error('Parameters are not enough.')
    else
    error('Symbol parameter is illegal.')
    end
elseif nargin == 5 & ~isstr(ybar)
    symbol='ob';
if length(x)~=length(xbar)
    error('Format of Xbar is illegal.')
elseif length(y)~=length(ybar)
    error('Format of Ybar is illegal.')
    end
elseif nargin == 5 & isstr(ybar)
    symbol=ybar;ybar=[];
if length(x)~=length(xbar)
    error('Format of Xbar is illegal.')
    end
    if sty(1) == 'v'
    ybar=xbar;xbar=[];
    elseif sty(1) == 'h'
    ybar=[];
    elseif sty(1) == 'd'
    error('Parameters are not enough.')
    else
    error('Symbol parameter is illegal.')
    end
    elseif nargin == 6
if length(x)~=length(xbar)
    error('Format of Xbar is illegal.')
elseif length(y)~=length(ybar)
    error('Format of Ybar is illegal.')
    end
if ~isstr(symbol)
    error('Symbol should be string')
    end
    end
    end


    [ls,col,mark,msg] = colstyle(symbol); if ~isempty(msg), error(msg); end
    symbol = [ls mark col]; % Use marker only on data part
    esymbol = ['-' col]; % Make sure bars are solid


    % 转换为列距阵
    [a,b]=size(x);
    if a < b
    x=x';y=y';xbar=xbar';ybar=ybar';
    c=a;a=b;b=c;
    end

    %% 处理上下限不等
    [xa,xb]=size(xbar);
    if xb==1
    ux=xbar;lx=xbar; 
    elseif xb==2
    lx=xbar(:,1);ux=xbar(:,2);
    end

    [ya,yb]=size(ybar);
    if yb==1
    uy=ybar;ly=ybar; 
    elseif yb==2
    ly=ybar(:,1);uy=ybar(:,2);
    end

    %% 描点
    dx=(max(x(:))-min(x(:)))/100;
    dy=(max(y(:))-min(y(:)))/100;
    logn=10;
    if length(sty) == 1
    xl = x-dx; xr = x+dx; yl = y-dy; yr = y+dy; % 定义端点短线长度
    plot(x,y,symbol);hold on
    elseif length(sty) == 5 & sty(2:5) == 'logx' 
    dx=(log(max(x(:)))-log(min(x(:))))/100;
    xl = x/logn^dx;xr = x*logn^dx;yl = y-dy; yr = y+dy; 
    semilogx(x,y,symbol);hold on
    elseif length(sty) == 5 & sty(2:5) == 'logy' 
    dy=(log(max(y(:)))-log(min(y(:))))/100;
    yl = y/logn^dy;yr = y*logn^dy;xl = x-dx; xr = x+dx; 
    semilogy(x,y,symbol);hold on
    elseif length(sty) == 5 & sty(2:5) == 'logd' 
    dx=(log(max(x(:)))-log(min(x(:))))/100;
    dy=(log(max(y(:)))-log(min(y(:))))/100;
    xl = x/logn^dx;xr = x*logn^dx; yl = y/logn^dy;yr = y*logn^dy;
    loglog(x,y,symbol);hold on
    end

    %% 纵向
    if sty(1) == 'v' | sty(1) == 'd'
    vx = zeros(a*9,b);
    vx(1:9:end,:) = x;
    vx(2:9:end,:) = x;
    vx(3:9:end,:) = NaN;
    vx(4:9:end,:) = xl;
    vx(5:9:end,:) = xr;
    vx(6:9:end,:) = NaN;
    vx(7:9:end,:) = xl;
    vx(8:9:end,:) = xr;
    vx(9:9:end,:) = NaN;

    vy = zeros(a*9,b);
    vy(1:9:end,:) = y-ly;
    vy(2:9:end,:) = y+uy;
    vy(3:9:end,:) = NaN;
    vy(4:9:end,:) = y-ly;
    vy(5:9:end,:) = y-ly;
    vy(6:9:end,:) = NaN;
    vy(7:9:end,:) = y+uy;
    vy(8:9:end,:) = y+uy;
    vy(9:9:end,:) = NaN;

    plot(vx,vy,esymbol,'markersize',20)
    end
    %% 横向
    if sty(1) == 'h' | sty(1) == 'd'
    hx = zeros(a*9,b);
    hx(1:9:end,:) = x-lx;
    hx(2:9:end,:) = x+ux;
    hx(3:9:end,:) = NaN;
    hx(4:9:end,:) = x-lx;
    hx(5:9:end,:) = x-lx;
    hx(6:9:end,:) = NaN;
    hx(7:9:end,:) = x+ux;
    hx(8:9:end,:) = x+ux;
    hx(9:9:end,:) = NaN;

    hy = zeros(a*9,b);
    hy(1:9:end,:) = y;
    hy(2:9:end,:) = y;
    hy(3:9:end,:) = NaN;
    hy(4:9:end,:) = yl;
    hy(5:9:end,:) = yr;
    hy(6:9:end,:) = NaN;
    hy(7:9:end,:) = yl;
    hy(8:9:end,:) = yr;
    hy(9:9:end,:) = NaN;

    plot(hx,hy,esymbol,'markersize',20)
    end
```

<h2 id="6">6. 一种matlab编程配置写法</h2>

配置文件
Initial.db
```
    0%%%%%%Configure
    0.01    
    0.99
    0.01
    0.99
    0%%%%%%getLine
    100
    15
    0
    30
    90
    50
    0%%%%%HSV
    0
    25
    170
    255
    30
    170
    30
    170
    0%%%%pile process cut
    100    %[100~200]
    0.8    %[0~1]
    0.73    %[0~1] 
    0.47   %[0~0.5]
```

![config][1]
读取部分的核心部分，最终返回一个x_coordinate.

```matlab
function [x_coordinate] = readConfTest(filename)
    RowNumber=getRowFromFile(filename); 
    f = fopen(filename) ; 
    tline = cell(1, RowNumber) ; 
    for i = 1 : RowNumber 
    tline{i} = fgetl(f);
    y = sscanf( tline{i} , '%f') ;
    x_coordinate(i)=y;
    end

    end


function row=getRowFromFile(filename)
    fid=fopen(filename,'rt'); % t是告诉fread是这里文本文件
    row=1;
while ~feof(fid)
    % 一次性读取10000字符，计算其中的回车个数，其中10是回车的ASCII编码
    % '*char'表示每次读取一个字符，*表示输出也是字符
    % 放心fread现在已经可以自动识别中文了，万一还是识别不了，
    % 请在fopen中指定文件编码格式，比如gbk
    row=row+sum(fread(fid,10000,'*char')==char(10));
    % 下面还有一个类似的方法，但是效率低很多，大概是上面的一半
    % 'char'表示每次读取一个字符，但是默认输出double，
    % 也就是说读取char然后转换double中间有转换能快吗？
    % row=row+sum(fread(fid,10000,'char')==10);
    end
    fclose(fid);
    %row=row+1;
    end

```

<h2 id="7">7. Cell and array</h2>

a. 调用cell数据的方式是{} 区分于array（使用[]创建array）的()调用数组数据。

[1]:/images/matlab/config.png
