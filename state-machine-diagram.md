# Family Feud State Machine

## States and Transitions

```
registration
  └─→ setup (automatic once two teams registered)

setup
  ├─→ countdown (via "Start Face-off")
  ├─→ round_over (via "Skip Face-off") 
  └─→ setup (via question navigation)

countdown (3 seconds)
  └─→ buzzer_active (automatic)

buzzer_active
  ├─→ faceoff (when team buzzes in)
  └─→ registration (via manual reset)

faceoff
  ├─→ main_game (when team answers correctly)
  ├─→ faceoff (strike reverses control)
  └─→ registration (via manual reset)

main_game  
  ├─→ main_game (correct answers, strikes 0-2)
  ├─→ steal (at 3 strikes)
  ├─→ round_over (when all answers revealed)
  └─→ registration (via manual reset)

steal
  ├─→ round_over (correct answer or strike)
  └─→ registration (via manual reset)

round_over
  ├─→ setup (via "Next Question")
  ├─→ setup (via "Previous Question") 
  └─→ registration (via manual reset)
```

## State Behaviors

### registration
- **Navigation**: Enabled (allows browsing questions while waiting for teams)
- **Answer Buttons**: Disabled
- **Face-off Controls**: Hidden
- **Display**: Shows team registration status, question preview
- **Auto-transitions**: setup when both teams registered

### setup
- **Navigation**: Disabled
- **Answer Buttons**: Disabled
- **Face-off Controls**: Show "Start Face-off" and "Skip Face-off"
- **Auto-transitions**: None

### countdown  
- **Navigation**: Disabled
- **Answer Buttons**: Disabled
- **Display**: Shows countdown timer
- **Auto-transitions**: buzzer_active after 3 seconds

### buzzer_active
- **Navigation**: Disabled
- **Answer Buttons**: Disabled  
- **Display**: "BUZZER ACTIVE!" message
- **Buzzer**: Active on player devices
- **Auto-transitions**: faceoff when team buzzes

### faceoff
- **Navigation**: Disabled
- **Answer Buttons**: Only controlling team enabled
- **Strike Behavior**: Reverses control, resets strikes to 0
- **Auto-transitions**: main_game on correct answer

### main_game
- **Navigation**: Disabled
- **Answer Buttons**: Only controlling team enabled
- **Strike Behavior**: Accumulates 0-3, triggers steal at 3
- **Auto-transitions**: steal at 3 strikes, round_over when board cleared

### steal
- **Navigation**: Disabled
- **Answer Buttons**: Only non-controlling team enabled
- **Strike Behavior**: Any strike ends round
- **Auto-transitions**: round_over on any answer (correct) or strike

### round_over
- **Navigation**: Enabled (can move to next question)
- **Answer Buttons**: Enabled for both teams (reveal only, no scoring)
- **Strike Button**: Disabled
- **Display**: Shows round results, next question preview
- **Auto-transitions**: None (admin chooses next action)
```