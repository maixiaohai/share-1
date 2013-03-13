### emacs配置 ###

##### python开发环境配置 #####
    ;;base
    ;;show line number
    (require 'linum)
    (global-linum-mode t)

    ;;yasnippet
    (add-to-list 'load-path "~/.emacs.d/yasnippet")
    (require 'yasnippet) ;; not yasnippet-bundle
    (yas--initialize)
    (yas/load-directory "~/.emacs.d/yasnippet/snippets")

    ;;auto-complete
    (add-to-list 'load-path "/home/wfc/.emacs.d/auto-complete")
    (require 'auto-complete-config)
    (add-to-list 'ac-dictionary-directories "/home/wfc/.emacs.d/auto-complete/ac-dict")
    (ac-config-default)

    ;;python-mode	
    (add-to-list 'load-path "~/.emacs.d/python-mode.el-6.1.1")
    (setq py-install-directory "~/.emacs.d/python-mode.el-6.1.1/")
    (require 'python-mode)

    ;;el-get
    (add-to-list 'load-path "~/.emacs.d/el-get/el-get")
    (unless (require 'el-get nil 'noerror)
    (with-current-buffer
	(url-retrieve-synchronously
		"https://raw.github.com/dimitri/el-get/master/el-get-install.el")
	(goto-char (point-max))
    		(eval-print-last-sexp)))
    (el-get 'sync)
    (autoload 'python-mode "python" "python Editing Mode" t)
    (setq interpreter-mode-alist (cons '("python2" . python-mode)
                                   interpreter-mode-alist))
				 (setq py-python-command-args '( "--colors" "Linux")
      py-smart-indentation t
      py-shell-name "python2"
      py-indent-offset 4)

    ;;jedi 是ipython补全的一个好的替代品
    (autoload 'jedi:setup "jedi" nil t)
    (add-hook 'python-mode-hook 'jedi:setup)

    ;;markdown
    (add-to-list 'load-path "~/.emacs.d/markdown/")
    (autoload 'markdown-mode "markdown-mode"
      "Major mode for editing Markdown files" t)
      (add-to-list 'auto-mode-alist '("\\.text\\'" . markdown-mode))







