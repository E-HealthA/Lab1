%% Lab - parte 1

Dataset = {};
%preallocate variable Dataset

url = 'https://itunes.apple.com/us/genre/ios-medical/id6020?mt=8&letter=A&page=1#page';
%definisco un url iniziale (di default sulla lettera A)

urlSplit = split(url,{'https://itunes.apple.com/us/genre/ios-medical/id6020?mt=8&letter=','&'});
%divido l'URL in 3: nella cell 2 trovo la pagina

alfabeto = ['A';'B';'C';'D';'E';'F';'G';'H';'I';'J';'K';'L';'M';'N';'O';'P';'Q';'R';'S';'T';'U';'V';'W';'X';'Y';'Z';'*'];
%'A';'B';'C';'D';'E';'F';'G';'H';'I';'J';'K';'L';'M';'N';'O';'P';'Q';'R';'S';'T';'U';'V';'W';'X';'Y';'Z';'*'
%creo un vettore che contiene tutte le lettere dell'alfabeto

for i = 1:length(alfabeto)
    
    urlSplit{2}=alfabeto(i);
    %vado a mettere nella cell 2 la lettera dell'alfabeto di cui voglio
    %trovare le app
    
    urlAttached = strjoin(urlSplit,{'https://itunes.apple.com/us/genre/ios-medical/id6020?mt=8&letter=','&'});
    %'riattacco' l'URL insieme
    
    S1 = webread(urlAttached);
    %leggo l'URL
        
    %cerco il numero delle pagine
    pattern = ' <ul class="list paginate"><li><a(.*?)</a></li></ul>';
    %prendo tutto ciò che c'e in mezzo con (.*?).
    
    URLnumero = regexp(S1, pattern,'match');
    %creo array con tutti gli URL
    
    if(isempty(URLnumero) == 1) 
        numeroPagine = 1;
    else
        arrayNumeri = split(URLnumero(1),{'#page">','</a></li>'});
        numeroChar = arrayNumeri{length(arrayNumeri)-2};
        numeroPagine = str2double(numeroChar);

        if (numeroPagine == 18)

            urlSplit18 = split(urlAttached,{'&page=','#page'});
            %spezzo di nuovo l'URL per andare a inserire i numeri

            urlSplit18{2}='18'; %%%%%%%%devo concertirlo in char
            %sostituisco j

            urlAttached18 = strjoin(urlSplit18,{'&page=','#page'});
            %riattacco l'URL

            S18 = webread(urlAttached18);
            %leggo l'URL

            pattern18 = ' <ul class="list paginate"><li><a(.*?)</a></li></ul>';
            %prendo tutto ciò che c'e in mezzo con (.*?).

            URLnumero18 = regexp(S18, pattern18,'match');
            %creo array con tutti gli URL

            arrayNumeri18 = split(URLnumero18(1),{'#page">','</a></li>'});
            numeroChar18 = arrayNumeri18{length(arrayNumeri18)-2};
            numeroPagine = str2double(numeroChar18);

            if (numeroPagine == 26)

                urlSplit26 = split(urlAttached18,{'&page=','#page'});
                %spezzo di nuovo l'URL per andare a inserire i numeri

                urlSplit26{2}='26'; %%%%%%%%devo concertirlo in char
                %sostituisco j

                urlAttached26 = strjoin(urlSplit26,{'&page=','#page'});
                %riattacco l'URL

                S26 = webread(urlAttached26);
                %leggo l'URL

                pattern26 = ' <ul class="list paginate"><li><a(.*?)</a></li></ul>';
                %prendo tutto ciò che c'e in mezzo con (.*?).

                URLnumero26 = regexp(S26, pattern26,'match');
                %creo array con tutti gli URL

                arrayNumeri26 = split(URLnumero26(1),{'#page">','</a></li>'});
                numeroChar26 = arrayNumeri26{length(arrayNumeri26)-2};
                numeroPagine = str2double(numeroChar26);

            end

        end
    end
    
    
    
    for j = 1:numeroPagine
        
        urlSplitNumeri = split(urlAttached,{'&page=','#page'});
        %spezzo di nuovo l'URL per andare a inserire i numeri
        
        urlSplitNumeri{2}=num2str(j); %%%%%%%%devo concertirlo in char
        %sostituisco j
        
        urlAttached2 = strjoin(urlSplitNumeri,{'&page=','#page'});
        %riattacco l'URL
        
        S = webread(urlAttached2);
        %leggo l'URL
        
        pattern = '<li><a href="https://itunes.apple.com/us/app/(.*?)</a> </li>';
        %prendo tutto ciò che c'e in mezzo con (.*?). Riesco a prendere cosi tutti gli URL
        
        URLapps = regexp(S, pattern,'match');
        %creo array con tutti gli URL
        
        
        for k=1:length(URLapps)
            
            splitURL = split(URLapps(k),'/');
            
            ID = split(splitURL(7),{'id','?'});
            Dataset(1,end+1) = ID(2);
            
            Name = split(ID(3),{'>','<'});
            Dataset(2,length(Dataset)) = Name(2);
            
        end
    end
end

Dataset = Dataset';


