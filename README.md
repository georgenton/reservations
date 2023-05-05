# Challenge de reservaciones (Tipti)

## Descripción del proyecto

### INTRODUCTION TO THE PROBLEM

The application must run. You should provide sufficient evidence that your solution is complete by, as a minimum, indicating that it works correctly against the supplied test data. Please note that you will be assessed on your judgment as well as your execution. The look and feel of the app are optional to you

### HOTEL RESERVATION PROBLEM

A hotel chain operating in Miami wishes to offer room reservation services over the internet.
They have three hotels in Miami: Lakewood, Bridgewood and Ridgewood. Each hotel has
separate weekday and weekend (Saturday and Sunday) rates. There are special rates for
rewards customer as a part of loyalty program. Each hotel has a rating assigned to it.
● Lakewood with a rating of 3 has weekday rates as 110$ for regular customer and 80$
for rewards customer. The weekend rates are 90$ for regular customer and 80$ for a
rewards customer.
● Bridgewood with a rating of 4 has weekday rates as 160$ for regular customer and 110$
for rewards customer. The weekend rates are 60$ for regular customer and 50$ for a
rewards customer.
● Ridgewood with a rating of 5 has weekday rates as 220$ for regular customer and 100$
for rewards customer. The weekend rates are 150$ for regular customer and 40$ for a
rewards customer.

### SOLUCIÓN PROPUESTA

- Repositorio de código fuente https://github.com/georgenton/reservations
- URL de prueba donde se despliega la aplicación https://desarrollo.live/

Debido a la optimización de tiempo, se decide utilizar el utilitatio vue3-datepicker
https://icehaunter.github.io/vue3-datepicker/
```
        Start Date
        <datepicker v-model="dateStart"></datepicker>

        End Date
        <datepicker v-model="dateEnd"></datepicker>
```
Se utilizó:
- VueJS3
- Typescript (básico)
- scss
```
<style lang="scss">
@import '@/assets/scss/variables.scss';
</style>
```

Se asume que llega información desde BackEnd de la siguiente manera

```
      dataHotels: [
        {
          id: 1,
          name: 'Lakewood',
          image: 'https://picsum.photos/id/14/300/400',
          regularWeekdayPrice: 100,
          rewardsWeekdayPrice: 80,
          regularWeekendPrice: 90,
          rewardsWeekendPrice: 80,
          rating: '3.0',
          rewards: true,
        },
        .....
```

Se crea un componente en forma de card que mostrará la información filtrada por las diferentes acciones

```
      <Hotel v-for="hotel in listHotels"
             :key="hotel.id"
             :name="hotel.name"
             :image="hotel.image"
             :price="hotel.price"
             :rewards="hotel.rewards"
             :weekday="hotel.weekday"
             :rating="hotel.rating"
      ></Hotel>
```
Se crea una función loadHoteles que toma la información en la que se filtra si las selecciones por default de "Rewards", "Regular", "Weekday" y "Weekend"

```
    loadHotels() {
      this.listHotels.splice(0, this.listHotels.length);
      this.dataHotels.forEach((hotel) => {
        switch (this.rewards) {
          case true: {
            switch (this.weekday) {
              case true: {
                const tmpHotel = {
                  id: hotel.id,
                  name: hotel.name,
                  image: hotel.image,
                  price: hotel.rewardsWeekdayPrice,
                  rating: hotel.rating,
                  rewards: true,
                  weekday: true,
                };
                this.listHotels.push(tmpHotel);
                break;
              }
              ...
```
Se crea una función que escoge el precio mas bajo

```
    bestPrice() {
      const tempHotel = this.listHotels;
      tempHotel.sort((a, b) => a.price - b.price);
      tempHotel.splice(1, this.listHotels.length);
      this.listHotels = tempHotel;
    },
```
Se crea dos Watchers para verificar que el dia seleccionado sea parte del weekday o del weekend
```
    dateStartChanged() {
      if (String(this.dateStart).search('Sat') === 0 || String(this.dateStart).search('Sun') === 0) {
        this.weekday = false;
      } else {
        this.weekday = true;
      }
      this.loadHotels();
    },
```

- Repositorio de código fuente https://github.com/georgenton/reservations
- URL de prueba donde se despliega la aplicación https://desarrollo.live/

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

