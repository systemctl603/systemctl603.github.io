My name is Arjun, and I am a developer who mostly uses web technologies such as:
* JavaScript
* HTML
* Markdown
* CSS

Some languages I wish to learn are:
* Python
* C/C++/C#
* Kotlin
* Rust
* Assembly 

Some codes I have made are this COVID-19 API:

```javascript
var stateData;
var covidData;
let coords;

function generateTableHead(table, data) {
  let thead = table.createTHead();
  let row = thead.insertRow();
  for (let key of data) {
    let th = document.createElement("th")
    let text = document.createTextNode(key)
    th.appendChild(text);
    row.appendChild(th);
  }
}

function generateTable(table, data) {
  for (let element of data) {
    let row = table.insertRow();
    for (let key in element) {
      let cell = row.insertCell();
      let text = document.createTextNode(element[key]);
      cell.appendChild(text);
    }
  }
}

async function main() {
  await $.getJSON('https://api.covid19india.org/state_district_wise.json', data => {
      console.log(data);
      covidData = data;
    })
    .fail(() => {
      document.getElementById("stat").innerHTML =
      `Oops! There was an error when fetching the API data.`;
    })

  let currentState;
  await $.getJSON('https://api.ipstack.com/check?access_key=<API KEY>', data => {
      currentState = data.region_name;
    })
    .fail(() => {
      document.getElementById("stat").innerHTML =
      `Oops! There was an error when locating the client.`;
    })


  console.log(covidData);

  let showndata
    showndata = covidData[currentState]['districtData'];
    if (showndata = undefined) {
    document.getElementById("stat").innerHTML =
    `Oops! There was an error when indexing the API. Error: ${error.message}`;
  }
  let array1 = [];
  let names = [];
  for (var prop in showndata) {
    names.push(prop);
  }
  let index = 0;
  let total = 0;
  Object.values(showndata).forEach((entry) => {
    array1.push({
      name: names[index],
      active: entry.active,
      confirmed: entry.confirmed,
      recovered: entry.recovered,
      deceased: entry.deceased
    })
    total += entry.confirmed;
    console.log(total);
    index++;
  })
  console.log(array1);


  let table = document.querySelector("table");
  let data = ["District", "Active", "Confirmed", "Recovered", "Deceased"]
  generateTableHead(table, data);
  generateTable(table, array1);
  if (total >= 1250) {
    document.getElementById("stat").innerHTML =
        `In this state, the amount of COVID-19 is <var class=\"severe\">very high</var>.
        Please stay indoors. Total: ${total}`;
  } else if (total >= 750) {
    document.getElementById("stat").innerHTML =
        `In this state, the amount of COVID-19 is <var class=\"medium\">less severe</var>.
        You still must exercise caution, but it is more lenient in
        this category. Total: ${total}`;
  } else if (total >= 350) {
    document.getElementById("stat").innerHTML =
        `In this state, the amount of COVID-19 is <var class=\"fine\">ok</var>.
        You must still stay inside to prevent further spread.
        Total: ${total}`;
  } else {
    document.getElementById("stat").innerHTML =
        `This state has managed to get their COVID-19 spread down.
        This is <var class=\"fine\">very good</var>. Self isolation will still help further.
        Total: ${total}`;
  }
}

main();

```
This OS kernel:

> kernel.c

```c
#define VGA_ADDRESS 0xB8000

#define BLACK 0
#define GREEN 2
#define YELLOW 14
#define WHITE 15

unsigned short *terminal_buffer;
unsigned int vga_index;

void clearScreen()
{
    int index = 0;
    while (index < 80 * 25 * 2)
    {
        terminal_buffer[index] = ' ';
        index += 2;
    }
}

void printString(char *str, unsigned char color)
{
    int index = 0;
    while (str[index])
    {
        terminal_buffer[vga_index] = (unsigned short)str[index] | (unsigned short)color << 8;
        index++;
        vga_index++;
    }
}
void main(void)
{
    terminal_buffer = (unsigned short *)VGA_ADDRESS;
    vga_index = 0;

    clearScreen();
    printString("Ambika Senthilvel", YELLOW);
    vga_index = 80;
    printString("Arjun Senthilvel", GREEN);
    vga_index = 160;
    printString("Arun Senthilvel", WHITE);
    return;
}
```

> boot.asm

```
bits 32

section .multiboot
    dd 0x1BADB002
    dd 0x0

    dd - (0x1BADB002 + 0x0)

section .text
global start
extern main

start:
    cli
    mov esp, stack_space
    call main
    hlt

section .bss
resb 8192
stack_space:
```

`
