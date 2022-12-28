# Growtopia-Captcha-Solver

## Parse Captcha UID


Varlist

```txt
param 0: onShowCaptcha
param 1: add_puzzle_captcha|0098/captcha/generated/576f9518-615c-4308-8d04-e6fc0c8fb905-PuzzleWithMissingPiece.rttex|0098/captcha/generated/576f9518-615c-4308-8d04-e6fc0c8fb905-TrimmedPuzzlePiece.rttex|ubistatic-a.akamaihd.net|200118|
end_dialog|puzzle_captcha_submit||Submit|
576f9518-615c-4308-8d04-e6fc0c8fb905-PuzzleWithMissingPiece.rttex -> Puzzle UID = 576f9518-615c-4308-8d04-e6fc0c8fb905
Puzzle UID = 576f9518-615c-4308-8d04-e6fc0c8fb905
```

Example Solver In C#
```chsarp
if (VarListFetched.Name == "onShowCaptcha" || VarListFetched.Name == "OnShowCaptcha")
        {
            Stopwatch stop = Stopwatch.StartNew();
            var OnShowCaptchaMSG = (string)VarListFetched.Args[1];
            var splitted = OnShowCaptchaMSG.Split("|");
            var captchaid = splitted[4];
            var captcha = splitted[1].Replace("0098/captcha/generated/", "");
            captcha = captcha.Replace("-PuzzleWithMissingPiece.rttex", "");
            _bot.BotLog.Append("Solving captcha: " + captcha, BotLog.LogType.Plutonium);
            Console.WriteLine("Solving captcha: " + captcha);
            var api = "http://45.83.246.197/captcha.php?captcha=" + captcha; // captcha solver
            var answer = new WebClient().DownloadString(api);
            if (!answer.Contains("Failed"))
            {
                _bot.SendPacket(2,
                    "action|dialog_return\ndialog_name|puzzle_captcha_submit\ncaptcha_answer|" + answer +
                    "|CaptchaID|" + captcha);
                _bot.BotLog.Append($"Solved captcha: {answer}", BotLog.LogType.Plutonium);
                Console.WriteLine($"Solved captcha: {answer}\nCaptcha was solved in {stop.ElapsedMilliseconds}ms\n");
            }
            else
            {
                _bot.Disconnect();
                _bot.BotLog.Append("CaptchaID : " + captchaid, BotLog.LogType.Plutonium);
                _bot.BotLog.Append("Captcha : " + captcha, BotLog.LogType.Plutonium);
                _bot.BotLog.Append("Bot Disconnected because of captcha failure", BotLog.LogType.Plutonium);
            }

            return;
        }
```

Example Solver In C++
```c++
case fnv32("onShowCaptcha"): {
          auto menu = varlist[1].get_string();
              if (menu.find("`wAre you Human?``") != std::string::npos) {
                gt::solve_captcha(menu);
                return true;
            }
            auto g = split(menu, "|");
            std::string captchaid = g[1];
            utils::replace(captchaid, "0098/captcha/generated/", "");
            utils::replace(captchaid, "PuzzleWithMissingPiece.rttex", "");
            captchaid = captchaid.substr(0, captchaid.size() - 1);

            http::Request request{ "http://45.83.246.197/captcha.php?captcha=" + captchaid };
            const auto response = request.send("GET");
            std::string output = std::string{ response.body.begin(), response.body.end() };
            g_server->send(false, "action|dialog_return\ndialog_name|puzzle_captcha_submit\ncaptcha_answer|" + output + "|CaptchaID|" + g[4]);
            return true;//success
        } break;
```

### Api
http://45.83.246.197/captcha.php?captcha=[CaptchaUID]

### Information
Solve Time 0-1.4 Seconds.<br>


### Price Information

<strong>ITS FREE! </strong>

<a href="https://discord.gg/N4jKYcKv">Discord Server</a>


### Example Request
Request Method : GET

```curl "http://45.83.246.197/captcha.php?captcha=07ed133c-ee0e-4fcb-8e76-81dda6aa5333" ```
### Response
Response txt format
```txt
0.29571
```
The result is the answer

discord Suryoyo#2808

### Too lazy to create a preview but it works!
