# Angular Pipe (or just Javascript) to Convert 24 Time to 12 Hour Format

I’m storing time in my database as 24 hour format (ie: “13:00” [1:00 PM]) because it’s easy and has a bunch of advantages. But on the front-end, in  [Angular](https://angular.io/), I want to show these as a 12 hour format. One way to do this is with a  [Pipe](https://angular.io/guide/pipes)  that will allow you to transform the time to 12 hour. Before creating the below I looking for something existing, but didn’t find anything that seemed to fit and work well, so below is what I use.

This is the function in the pipe (can be used in Javascript for those w/o Angular): (try in  [jsfiddle](https://jsfiddle.net/6adh4ukv/))

```
var time24To12 = function(a) {
    //below date doesn't matter.
    return (new Date("1955-11-05T" + a + "Z")).toLocaleTimeString("bestfit", {
        timeZone: "UTC",
        hour12: !0,
        hour: "numeric",
        minute: "numeric"
    });
};
```

When called and passed a time, like:

```
time24To12('13:00')
```

It will return the 12 hour version:

```
1:00 PM
```

## OKAY, SO HOW TO ADD A NEW PIPE AND USE IT IN ANGULAR?

1.  Create a new file “**time24to12.pipe.ts**“
2.  File contents should be the below:

```
import { Pipe, PipeTransform } from '@angular/core';


@Pipe({ name: 'time24to12Transform' })
export class Time24to12Format implements PipeTransform {
  transform(time: any): any {
    
 var time24To12 = function(a) {
    //below date doesn't matter.
    return (new Date("1955-11-05T" + a + "Z")).toLocaleTimeString("bestfit", {
        timeZone: "UTC",
        hour12: !0,
        hour: "numeric",
        minute: "numeric"
    });
};

    return time24To12(time); 
  }
}

```

3. Make sure in your  **app.module.ts**  you import this file:

```
import { Time24to12Format } from './time24to12.pipe';
```

…and also add it to the  **declarations**  section:

```
@NgModule({
  declarations: [
   ...
    Time24to12Format,
 ...
```

4. Now you can use it in views, like below:

```
{{yourobject.time | convertFrom24To12Format}}
```

Hope this helps someone. No, this code doesn’t check beforehand for correct time input formatting or perform many other things it should, it just trusts you.
