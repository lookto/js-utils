# Validating an International Securities Identification Number (ISIN)

The [ISIN](https://en.wikipedia.org/wiki/International_Securities_Identification_Number#Description) is a 12-character alphanumeric code that is used to identify fungible securities like equities or derivatives.

For validation the ISIN needs to be converted from base36 to base10 and then be validated using the [Luhn algorithm](https://en.wikipedia.org/wiki/Luhn_algorithm).

``` js
export const isValidISIN = (isin: string) => {
    let regex =
        /^(AD|AE|AF|AG|AI|AL|AM|AO|AQ|AR|AS|AT|AU|AW|AX|AZ|BA|BB|BD|BE|BF|BG|BH|BI|BJ|BL|BM|BN|BO|BQ|BR|BS|BT|BV|BW|BY|BZ|CA|CC|CD|CF|CG|CH|CI|CK|CL|CM|CN|CO|CR|CU|CV|CW|CX|CY|CZ|DE|DJ|DK|DM|DO|DZ|EC|EE|EG|EH|ER|ES|ET|FI|FJ|FK|FM|FO|FR|GA|GB|GD|GE|GF|GG|GH|GI|GL|GM|GN|GP|GQ|GR|GS|GT|GU|GW|GY|HK|HM|HN|HR|HT|HU|ID|IE|IL|IM|IN|IO|IQ|IR|IS|IT|JE|JM|JO|JP|KE|KG|KH|KI|KM|KN|KP|KR|KW|KY|KZ|LA|LB|LC|LI|LK|LR|LS|LT|LU|LV|LY|MA|MC|MD|ME|MF|MG|MH|MK|ML|MM|MN|MO|MP|MQ|MR|MS|MT|MU|MV|MW|MX|MY|MZ|NA|NC|NE|NF|NG|NI|NL|NO|NP|NR|NU|NZ|OM|PA|PE|PF|PG|PH|PK|PL|PM|PN|PR|PS|PT|PW|PY|QA|RE|RO|RS|RU|RW|SA|SB|SC|SD|SE|SG|SH|SI|SJ|SK|SL|SM|SN|SO|SR|SS|ST|SV|SX|SY|SZ|TC|TD|TF|TG|TH|TJ|TK|TL|TM|TN|TO|TR|TT|TV|TW|TZ|UA|UG|UM|US|UY|UZ|VA|VC|VE|VG|VI|VN|VU|WF|WS|YE|YT|ZA|ZM|ZW)([0-9A-Z]{9})([0-9])$/;
    let match = regex.exec(isin);

    if (match?.length != 4) return false;

    return Number(match[3]) === getLuhnChecksum(match[1] + match[2]);
};

const getLuhnChecksum = (input: string) => {
    let reversedInputBase10 = [];
    for (let i = input.length - 1; i >= 0; i--) {
        let char = input.charAt(i);

        if (isNaN(Number(char))) {
            let letterCode = input.charCodeAt(i) - 55;
            reversedInputBase10.push(letterCode % 10);
            if (letterCode > 9) {
                reversedInputBase10.push(Math.floor(letterCode / 10));
            }
        } else {
            reversedInputBase10.push(Number(char));
        }
    }

    let sum = 0;
    for (let i = 0; i < reversedInputBase10.length; i++) {
        if (i % 2 == 0) {
            let double = reversedInputBase10[i] * 2;
            sum += Math.floor(double / 10);
            sum += double % 10;
        } else {
            sum += reversedInputBase10[i];
        }
    }
    return (10 - (sum % 10)) % 10;
};

```