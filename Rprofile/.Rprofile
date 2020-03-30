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
set_defpacks <- function()
{
    args_list <- list('path' = c(file.path('G:', '03_Arbeiten', 'Projekte', 'Statistikprogramm R', 'sandbox', 'configs'),
                                 file.path('~', 'Documents', 'Rworkspace', 'sandbox', 'configs')),
                      'pattern' = '.Rprofile_defaultpackages',
                      'all.files' = TRUE,
                      'full.names' = TRUE)
    args_df <- as.data.frame(args_list, stringsAsFactors = FALSE)
    
    defpacks_files <- c(do.call('list.files', args_df[1,]),
                        do.call('list.files', args_df[2,]))

    if( length(defpacks_files) > 0 ) 
    {
        if( file.exists(defpacks_files[1]) )
        {
            defpacks_tbl <- utils::read.csv(defpacks_files[1], as.is = TRUE)
            if( nrow(defpacks_tbl) > 0)
            {
                options(defaultPackages = unique(c(getOption('defaultPackages'), defpacks_tbl$defaultPackages)))
                message('Additional defaultPackages set!')
            }
        }
    }
    return(invisible(NULL))
}

# Startup function
.First <- function()
{
	# set author
	author <- utils::person('Flavian', 'Imlig', email = 'flavian.imlig@bi.zh.ch')
	
	# set default packages
	set_defpacks()

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
