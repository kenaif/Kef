# Kef
Chatbot
//***********************************
//*			Install Modules			*
//*---------------------------------*
//*		Install with command		*
//*									*
//* npm install request				*
//* npm install facebook-chat-api	*
//*									*
//***********************************
// Facebook Chat API By Schmavery
// Git_URL: https://github.com/Schmavery/facebook-chat-api
// Code: Trunghieuth10 - trunghieuth10@gmail.com

// Khai báo
var request = require("request");
var login = require("facebook-chat-api");
var SimsimiAnswered;
var text;
var botkey = "http://www.simsimi.com/getRealtimeReq?uuid=UwmPMKoqosEETKleXWGOJ6lynN1TQq18wwvrmCy6IRt&lc=vn&ft=0&reqText=";
login(
	{	
	email: "k79pro@gmail.com", 
	password: "kenail 123 " 
	},
function callback (err, api)
{
	if(err) return console.error(err);
	
	api.setOptions({forceLogin: true, selfListen: false, logLevel: "silent"});
	
	api.listen(function callback(err, message)
	{
		if(message.body === "#Phonbestdeptrai"||message.senderID=== "#phonbestdeptrai") { 
			api.sendMessage("Okayyy...Bố cút'ss...Hứ", message.threadID); 
			api.markAsRead(message.threadID);
			return api.logout(err);
		}
		else if(message.body === "/help") { 
			console.log("FormID: " + message.threadID + '->Message: '+message.body);
			api.sendMessage("\n- Đôi lời từ Phon-sama \n- Nhắn tin kèm . ở đầu câu để tránh con bot ml tự động trả lời. \n- Nhắn tin bất kỳ để tiếp tục cuộc trò chuyện. \n- Gõ #Phondeptrai để con bot ml câm cmn mồm", message.threadID); 
			api.sendMessage("\n- Đôi lời từ Phon-sama \n- Nhắn tin kèm . ở đầu câu để tránh con bot ml tự động trả lời. \n- Nhắn tin bất kỳ để tiếp tục cuộc trò chuyện. \n- Gõ #Phondeptrai để con bot ml câm cmn mồm", message.threadID);
			return;
		}
		else if(message.body === "#Phondeptrai"||message.senderID=== "#phondeptrai") { 
			console.log("FormID: " + message.threadID + '->Message: '+message.body);
			api.sendMessage("Có cl ấy...tin người vkl... =)))", message.threadID); 
			api.sendMessage("\n- Đôi lời từ Phon-sama \n- Nhắn tin kèm . ở đầu câu để tránh con bot ml tự động trả lời. \n- Nhắn tin bất kỳ để tiếp tục cuộc trò chuyện. \n- Gõ #Phondeptrai để con bot ml câm cmn mồm", message.threadID);
			return;
		}
		else if (message.body.indexOf(".")===0){
			console.log("FormID: " + message.threadID + '->Message: '+message.body);
			return;
		}
		
		{
			console.log("FormID: " + message.threadID + '->Message: '+message.body);
			request(botkey + encodeURI(message.body),  
			function(error, response, body)
			{  
				if (error) api.sendMessage("Tao đang đơ, không trả lời được :)", message.threadID);
				if (body.indexOf("502 Bad Gateway") > 0 || body.indexOf("respSentence") < 0)
					api.sendMessage("Cái ji đây ?? :D ??: " + message.body, message.threadID 
				);
				text = JSON.parse(body);
				if (text.status == "200")
				{
					SimsimiAnswered = text.respSentence;
					if (message.body===text.respSentence) {
						return;
					} else
					SimsimiAnswered = text.respSentence;
					api.sendMessage(SimsimiAnswered+"\n \n --------\nTừ trợ lý của Phon-sama. \n- Gõ /help để được Phon-sama trợ giúp", message.threadID);
					api.markAsRead(message.threadID);
					console.log("Answered:"+SimsimiAnswered);
				}
			});
		}
	});
});
