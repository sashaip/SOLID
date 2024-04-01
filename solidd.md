
# Project Title

Ts Solid

Single responsibility-каждый класс должен быть ответственен за выполнение только одной конкретной задачи или иметь только одну причину для изменения.Это помогает сделать код более упрощенным.

При возникновении действий должен затрагиватся только один класс.

Пример: 
Первоначальный класс:

class Auto{
    constructor(model: string){}
    getCarModel(){}
    saveCustomerOrder(o: Auto){}
}

через некоторое время у нас появились новые сущности, и вот что будет с ним:

class Auto{
    constructor(model: string){}
    getCarModel(){}
    saveCustomerOrder(o: Auto){}
    setCarModel(){}
    getCutomerOrder(id: string){}
    removeCustomerOrder(id: string){}
    updateCarSet(set: object){}
}

Код превратился в что-то непонятное.Поэтому нам нужно разбить его на отдельные сущности:


class Auto{
    constructor(model: string){}
    getCarModel(){}
    setCarModel(){}
}

class CustomerAuto{
    saveCustomerOrder(o: Auto){}
    getCutomerOrder(id: string){}
    removeCustomerOrder(id: string){}
}

class AutoDB{
    updateCarSet(set: object){}
}


Open-Closed- это принцип открытости и закрытости,означает, что классы и функции должны быть открыты для расширения, но закрыты для изменения.


Пример:

class Auto{
    constuctor(public model: string){}
    getCarModel(){}
}

const shop: Array<Auto> = [
    new Auto("Tesla"),
    new Auto("Audi"),
];

const getPrice = (auto: Array<Auto>): string | void => {
    for (let i = 0; i < auto.length; i++){
        switch (auto[i].model){
            case 'Tesla': return '80.000$';
            case 'Audi': return '50.000$';
            default: return 'No Auto Price';
        }
    }
}

getPrice(shop);

Если добавить новую модель машины, то придется менять и "const getPrice",по этой причине, принцп не будет работать.

Нам нужно сделать следующее:

abstract class CarPrice{
    abstact getPrice(): string;
}

class Tesla extends CarPrice{
    getPrice(){
        return '80.000$';
    }
}

class Audi extends CarPrice{
    getPrice(){
        return '50.000$';
    }
}

class Bmw extends CarPrice{
    getPrice(){
        return '70.000$';
    }
}

Меняем "const getPrice"

const shop: Array<Auto> = [
    new Auto("Tesla"),
    new Auto("Audi"),
    new Auto("Bmw")
];

const getPrice = (auto: Array<Auto>): string | void => {
    for (let i = 0; i < auto.length; i++){
        auto[i].getPrice();
    }
}

getPrice(shop);

Liskov Substitution-принцип подстановки Барбары Лисков.Подклассы должны быть взаимозаменяемы с их базовым классом без изменения ожидаемого поведения программы.

Пример:

class Rectangle {
    constructor(public width: number, public heigth: number)

    setWidth(width: number){
        this.width = width;
    }

    setHeigth(heigth: number){
        this.heigth = heigth;
    }

    areaOf(): number {
        return this.width * this.heigth
    }
}

class Square extends Rectangle{
    width: number = 0;
    heigth: number = 0;

    constructor(size: number){
        super(size, size)
    }

    setWidth(width: number){
        this.width = width;
        this.heigth = heigth;
    }

    setHeigth(heigth: number){
        this.heigth = heigth;
        this.width = width;
    }
}

const mySquare = new Square(20);
mySquare.setWidth(30);
mySquare.setWidth(40);


Interface Segregation- разделение интерфейса,означает, что клиенты не должны зависеть от интерфейсов, которые они не используют.

Расмотрим на примере кода:

interface AutoSet{
    getTeslaSet(): any;
    getAudiSet(): any;
    getBmwSet(): any;
}

class Tesla implements AutoSet{
    getTeslaSet(): any;
    getAudiSet(): any;
    getBmwSet(): any;
}

class Audi implements AutoSet{
    getTeslaSet(): any;
    getAudiSet(): any;
    getBmwSet(): any;
}

class Bmw implements AutoSet{
    getTeslaSet(): any;
    getAudiSet(): any;
    getBmwSet(): any;
}

interface TeslaSet{
    getTeslaSet(): any;
}

interface AudiSet{
    getAudiSet(): any;
}

interface BmwSet{
    getBmwSet(): any;
}

class Tesla implements TeslaSet{
    getTeslaSet(): any;
}

class Tesla implements TeslaSet{
    getAudiSet(): any;
}

class Tesla implements TeslaSet{
    getBmwSet(): any;
}
