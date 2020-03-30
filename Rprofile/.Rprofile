# user R configurations template
#
# manual site configuration: place Rprofile.site in R_HOME directory
# manual user configuration: place .Rprofile in R_USER directory
# manual project configuration: place .Rprofile in working/project directory
#
# Author: Flavian Imlig <flavian.imlig@bi.zh.ch>
# Date: 30.03.2019
###############################################################################

# set UTF-8 to be default encoding
# options(encoding = 'UTF-8')

# set a CRAN mirror
local({r <- getOption("repos")
r["CRAN"] <- "https://stat.ethz.ch/CRAN"
options(repos=r)})

# set KTZH Proxy
Sys.setenv(http_proxy="proxy.kt.ktzh.ch:8080")
Sys.setenv(https_proxy="proxy.kt.ktzh.ch:8080")

# function to set additional default packages
.set_defpacks <- function(load = TRUE)
{
    # no load option
    if( isFALSE(load) ) {
        message('No additional defaultPackages set in .Rprofile! (load = FALSE)')
        return(invisible(NULL))
    }
    
    # table locations
    file <- '.Rprofile_defaultpackages'
    paths <- list(url('http://raw.githubusercontent.com/bildungsmonitoringZH/Rconfig/master/Rprofile/.Rprofile_defaultpackages'),
                  file.path(Sys.getenv('R_USER'), file))
    
    # get package table
    tbl <- suppressWarnings(try(utils::read.csv(paths[[1]], as.is = TRUE), silent = TRUE))
    if( is(tbl, 'try-error') ) { close(paths[[1]]) }
    if( is(tbl, 'try-error') ) { tbl <- suppressWarnings(try(read.csv(paths[[2]], as.is = TRUE), silent = TRUE)) }
    
    
    # check package table, set R option defaultPackages
    if( is(tbl, 'data.frame') ) {
        if( identical(names(tbl), 'defaultPackages') & nrow(tbl) > 0 ) {
            options(defaultPackages = unique(c(getOption('defaultPackages'), tbl$defaultPackages)))
            return(invisible(NULL))
        }
    }
    
    # return on failure
    message('No additional defaultPackages set in .Rprofile! (failure)')
    return(invisible(NULL))
}

# Startup function
.First <- function()
{
    # set author
    author <- utils::person('Flavian', 'Imlig', email = 'flavian.imlig@bi.zh.ch')
    
    # set default packages
    .set_defpacks(load = TRUE)
    
    # set additional options
    if( 'devtools' %in% rownames(utils::installed.packages()) ) {
        options(devtools.desc.author = author)
    } else {
        message(sprintf('%s: Package %s not installed!', as.character(match.call()[[1]]), 'devtools'))
    }
    
    # load system fonts
    if( !'extrafont' %in% rownames(utils::installed.packages()) )
    {
        message(sprintf('%s: Package %s not installed!', as.character(match.call()[[1]]), 'extrafont'))
    } else {
        if( is.null(extrafont::fonts()) )
        {
            message('No fonts imported (extrafont package)!')
        } else {
            pdfFonts <- grDevices::pdfFonts
            extrafont::loadfonts('pdf', quiet = TRUE)
            windowsFonts <- grDevices::windowsFonts
            extrafont::loadfonts('win', quiet = TRUE)
            postscriptFonts <- grDevices::postscriptFonts
            extrafont::loadfonts('postscript', quiet = TRUE)
        }
    }
    
    # echo author
    message(sprintf('Profile for %s loaded.', author))
}
