# slack-message-cleaner
Delete Slack conversation messages

Run in web browser console:   

```javascript
var overEvent = new MouseEvent("mouseover", {
  bubbles: true,
  cancelable: true,
  view: window
})

var messages = document.querySelectorAll(".c-virtual_list__item")

function deleteMessage(messages, index) {
  console.log("deleteMessage")
  if (index >= messages.length) {
    console.log("index bigger than messages length")
    return
  }

  var message = messages[index]
  message.dispatchEvent(overEvent)
  setTimeout(function() {
    actionsButton = message.querySelector("[data-qa=more_message_actions]")
    if (actionsButton) {
      actionsButton.click()

      setTimeout(function() {
        deleteButton = document.querySelector("[data-qa=delete_message]")
        if (deleteButton) {
          setTimeout(function() {
            deleteButton.click()
            setTimeout(function() {
              confirmButton = document.querySelector("[data-qa=generic_dialog_go]")
              if (confirmButton) {
                confirmButton.click()
              }
              else {
                console.log("no confirmButton")
              }
              setTimeout(function() { deleteMessage(messages, index + 1) }, 100)
            }, 500)
          }, 500)
        }
        else {
          console.log("no deleteButton")
          setTimeout(function() { deleteMessage(messages, index + 1) }, 100)
        }
      }, 100)
    }
    else {
      console.log("no actionsButton")
      setTimeout(function() { deleteMessage(messages, index + 1) }, 100)
    }
  }, 100)
}

deleteMessage(messages, 0)
```
