const WHATSAPP_ACCESS_TOKEN = "your whatsapp access token from meta";
const WHATSAPP_TEMPLATE_NAME = "name of the template you chose";
const LANGUAGE_CODE = "en_US";
const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Name of your sheet");
let data = sheet.getDataRange().getValues();

function onOpen() {
   let ui = SpreadsheetApp.getUi();
  ui.createMenu('Send')
  .addItem('Send Email and Message', 'send')
}

function send(){
    email();
    whatsapp();
}

function email() {
  
  let subject = 'subject of email';
  let message = 'message you wanna send';
  for(let i = 1; i < data.length; i++){
    // here data[i][5] is the email address
    if(data[i][5] == '' ){
      break
    }
    // in here we are checking if the email and message is already sent or not if it is sent then the 7th column would be marked as sent.
    if((data[i][7] != 'sent') && (data[i][5] != '')){
      const email = (data[i][1]);
      MailApp.sendEmail(email,subject,message);
    }
  }
}

function number(num){
  // here data[i][3] = country code example india +91.
  // and data[1][4] = the number that you want to send message to.
  let number = data[num][3] + parseInt(data[num][4]);
  number = number.replace(/\D/g,'').trim()
  console.log(number)
  number = parseInt(number);
  console.log(number)
  return number;
}

const getSheetData_ = () => {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Name of your sheet");
  const [header, ...rows] = sheet.getDataRange().getDisplayValues();
  const data = [];
  rows.forEach((row) => {
    const recipient = {};
    header.forEach((title, column) => {
      recipient[title] = row[column];
    });
    data.push(recipient);
  });
  return data;
};


const sendMessage_ = ({
  recipient_number,
}) => {
  const apiUrl = 'enter the apiurl here for meta';
  const request = UrlFetchApp.fetch(apiUrl, {
    muteHttpExceptions: true,
    method: 'POST',
    headers: {
      Authorization: `Bearer ${WHATSAPP_ACCESS_TOKEN}`,
      'Content-Type': 'application/json',
    },
    payload: JSON.stringify({
      type: 'template',
      messaging_product: 'whatsapp',
      to: recipient_number,
      template: {
        name: WHATSAPP_TEMPLATE_NAME,
        language: { code: LANGUAGE_CODE},
        components: [
          {
            type: 'body'
          },
        ],
      },
    }),
  });

  const { error } = JSON.parse(request);
  const status = error ? `Error: ${JSON.stringify(error)}` : `Message sent to ${recipient_number}`;
  Logger.log(status);
};

const whatsapp = () => {
  getSheetData_().forEach((row) => {
    const status = sendMessage_({
      recipient_number: row['Phone Number'].replace(/[^\d]/g, ''),
    });
  });
  for(let i = 1; i < data.length; i++){
    // here data[i][5] is the email address
    if(data[i][5] == '' ){
      break
    }
    // in here we are checking if the email and message is already sent or not if it is sent then the 7th column would be marked as sent.
    if(data[i][7] != 'sent'){
      const status = sendMessage_({
        recipient_number: number(i),
      });
      // by using the line below we are setting the value of the column to sent after sending the message.
      SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Name of your sheet").getRange(i + 1, 8).setValue('sent');
      console.log(status);
    }
  }
};