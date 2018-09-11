# hypertables

/*
hypertable jquery plugin
Author: Derry Spann
description: jquery plugin that converts html tables into JSON data
*/
$.fn.hyperTable = function (options) {
    console.log('initialized hypertable plugin')
    checkType = function (htmlcontent) { /*check element type to for images,links,js*/
        var content = $(htmlcontent).html(); 
        try {
            if ($(content).is('img')) {
                imgobj = {
                    src: $(content).attr('src'),
                    title: $(content).attr('title')
                }
                return imgobj
            }
            else if ($(content).is('a')) {
                linkobj = {
                    text: $(content).attr('title'),
                    href: $(content).attr('href')
                }
                return linkobj;
            }
            else {
                contentObj = {
                    html:$(htmlcontent).html().trim(),
                    text:$(htmlcontent).text().trim()
                }
                return contentObj
            }
        }
        catch (err) {return $(htmlcontent).text().trim(); }
    }
    var collection = [], headings = [], dataObj = {}; 
        this.find('tr:first-child').addClass('datakeys').find('th,td').each(function (index, value) {headings.push($(value).text().trim().toLowerCase().replace(/ /g,'_')); }); 
    if (options === 'object') {
        this.find('tr').each(function (indx, obj) {dataObj[$(this).find('td:first-child').text().trim().replace(/ /g,'_')] = checkType($(this).find('td:last-child')); }); return dataObj; 
    } else {
        this.find('tr.datakeys').siblings().each(function () {
        	if (!$(this).hasClass('ignore')) {$(this).addClass('data'); } 
        }); 
    	this.find('tr.data').each(function (indx, obj) {var data = {}; 
	    	$(obj).children('td').each(function (id, elem) {
	    		data[headings[id].toLowerCase().replace(' ', '_')] = checkType($(elem)); delete(data['']); 
	    	}); 
	    	collection.push(data); 
    	}); 
    	this.remove();
    	return collection; 
    }
}; // end hypertable

