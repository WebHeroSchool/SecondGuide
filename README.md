# SecondGuide
ЛУЧШИЕ ПРАКТИКИ

1. Осмысленно называть переменные. Это уменьшит необходимость в дополнительных комментариях, так код говорит сам за себя.

Так делать не надо:

cosnt ddmmyyyy = new Date();

 А это хорошо:

const date = new Date();



2. Важно, чтобы код был читаемым и доступным для поиска. Если по названию значения не понятно, что оно делает или должно делать, это сбивает с толку.

Так делать не надо:

setTimeout(randomFunction, 86400000);

 А это хорошо:

const MilliSecondsInDay = 86_400_000;
setTimeout(blastOff, MilliSecondsInDay);



3. Контекст переменных делать такой, чтобы их можно было понимать даже тогда, когда не удалось проследить всю историю их возникновения.

Так делать не надо:

const names = ["John", "Jane", "Joseph"];
names.forEach(v => {
 doStuff();
 doSomethingExtra();
 // ...
 // ...
 // ...
 // What is this 'v' for?
 dispatch(v);
});

 А это хорошо:

const names = ["John", "Jane", "Joseph"];
names.forEach(name => {
 doStuff();
 doSomethingExtra();
 // ...
 // ...
 // ...
 // 'name' makes sense now
 dispatch(name);
});



4. Если имя класса или объекта говорит вам, что это такое, не включайте это в имя переменной.

Так делать не надо:

const Book = {
 bookName: "Programming with JavaScript",
 bookPublisher: "Penguin",
 bookColour: "Yellow"
};
function wrapBook(book) {
 book.bookColour = "Brown";
}

 А это хорошо:

const Book = {
 name: "Programming with JavaScript",
 publisher: "Penguin",
 colour: "Yellow"
};
function wrapBook(book) {
 book.colour = "Brown";
}



5. Чтобы получить более читабельный код, лучше установить параметрам значения по умолчанию.

Так делать не надо:

function addEmployeeType(type){
 const employeeType = type || "intern";
 //............
}

 А это хорошо:

function addEmployeeType(type = "intern"){
 //............
}


6. Использовать “===” вместо “==” . Это поможет избежать всяких ненужных проблем в дальнейшем. Если не делать проверки должным образом, то это может существенно повлиять на логику программы.

 0 == false // true
 0 === false // false
 2 == "2" // true
 2 === "2" // false



7. Использовать описательные имена, которые говорят сами за себя. Рассматривать функции, которые представляют определенное поведение, имя функции должно быть глаголом или словосочетанием, полностью раскрывающим намерение, стоящее за ним, а также намерение аргументов. Их имена должны говорить о том, что они делают.

Так делать не надо:

function sMail(user){
 //........
}

 А это хорошо:

function sendEmail(emailAddress){
 //.........
}



8. В идеале следует избегать большого количества параметров. Уменьшение количества параметров функции облегчает ее тестирование.
Один или два аргумента - идеальный вариант, а три следует по возможности избегать. Все, что больше этого, должно быть сведено воедино. Обычно, если более двух аргументов, то функция пытается сделать слишком много. В тех случаях, когда это не так, в большинстве случаев в качестве аргумента будет достаточно высокоуровненового объекта.

Так делать не надо:

function createMenu(title, body, buttonText, cancellable) {
 // ...
}
createMenu("Foo", "Bar", "Baz", true);

 А это хорошо:

function createMenu({ title, body, buttonText, cancellable }) {
 // ...
}
createMenu({
 title: "Foo",
 body: "Bar",
 buttonText: "Baz",
 cancellable: true
});



9. Функции должны делать что-то одно. Это одно из важнейших правил в программировании. Если функция выполняет более, чем одну вещь, то ee сложнее проверить и отладить. Если функция изолированна и делает что-то одно, ее проще отрефакторить.

Так делать не надо:

function notifyListeners(listeners) {
 listeners.forEach(listener => {
  const listenerRecord = database.lookup(listener);
  if (listenerRecord.isActive()) {
   notify(listener);
  }
 });
} 

А это хорошо:

function notifyActiveListeners(listeners) {
 listeners.filter(isListenerActive).forEach(notify);
}

function isListenerActive(listener) {
 const listenerRecord = database.lookup(listener);
 return listenerRecord.isActive();
}



10. Удалять дубликат кода. Писать один и тот же код более одного раза не только бесполезно, но и еще приведет к проблемам при рефакторинге кода. Вместо того, чтобы одно изменение повлияло на все соответствующие модули, надо найти все дубликаты модулей и повторить это изменение.
Часто дублирование в коде происходит потому, что два или более модуля имеют небольшие различия, так как есть две или более немного отличающихся сущностей, которые имеют много общего.
Небольшие различия заставляют иметь очень похожие модули. Удаление дублирующего кода означает создание абстракции, которая может обрабатывать этот набор различных сущностей с помощью одной функции/модуля/класса.

Так делать не надо:

function showDeveloperList(developers) {
 developers.forEach(developer => {
  const expectedSalary = developer.calculateExpectedSalary();
  const experience = developer.getExperience();
  const githubLink = developer.getGithubLink();
  const data = {
   expectedSalary,
   experience,
   githubLink
  };
  render(data);
 });
}
 function showManagerList(managers) {
 managers.forEach(manager => {
  const expectedSalary = manager.calculateExpectedSalary();
  const experience = manager.getExperience();
  const portfolio = manager.getMBAProjects();
  const data = {
   expectedSalary,
   experience,
   portfolio
  };
  render(data);
 });
} 

А это хорошо:

function showEmployeeList(employees) {
 employees.forEach(employee => {
  const expectedSalary = employee.calculateExpectedSalary();
  const experience = employee.getExperience();
  const data = {
   expectedSalary,
   experience
  };
  switch (employee.type) {
   case "manager":
    data.portfolio = employee.getMBAProjects();
    break;
   case "developer":
    data.githubLink = employee.getGithubLink();
    break;
  }
  render(data);
 });
}
