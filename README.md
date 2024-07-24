# project-fishes

import {controller, target, targets} from '@github/catalyst'

@controller
export class LoadingContextElement extends HTMLElement {
  @target spinner: HTMLElement
  @target details: HTMLElement
  @targets fragments: HTMLElement[]

  #observer: MutationObserver

  get hasLoadingFragments() {
    return (
      this.fragments
        .filter(fragment => !fragment.classList.contains('is-error'))
        .filter(fragment => fragment.hasAttribute('src')).length > 0
    )
  }

  connectedCallback() {
    if (!this.hasLoadingFragments) {
      return this.showDetails()
    }

    this.#observer = new MutationObserver(() => {
      if (!this.hasLoadingFragments) {
        this.showDetails()
        this.#observer.disconnect()
      }
    })

    this.#observer.observe(this.details, {childList: true, attributes: true, subtree: true})
  }

  private showDetails() {
    this.spinner.remove()
    this.details.toggleAttribute('hidden', false)
  }
}

mport {attr, controller, target} from '@github/catalyst'
import type {ModalDialogElement} from '@primer/view-components/app/components/primer/alpha/modal_dialog'

@controller
class DeferredSidePanelElement extends HTMLElement {
  @attr url: string
  @target fragment: HTMLElement
  @target panel: ModalDialogElement

  loadPanel() {
    // the fragment target gets removed from the document once the panel has loaded,
    // so we don't need to run setup again even if this element is reconnected
    if (!this.fragment || this.fragment.hasAttribute('data-loaded')) return

    this.fragment.setAttribute('src', this.url)

    this.fragment.addEventListener('include-fragment-replace', event => {
      if (event instanceof CustomEvent && event.detail.fragment) {
        const newFragment = event.detail.fragment
        const oldDialog = this.fragment.querySelector('modal-dialog, dialog')
        const newDialog = newFragment.querySelector('modal-dialog, dialog')
        if (!oldDialog || !newDialog) return

        // manually swap out the dialog contents,
        // injecting the new dialog children into the old dialog
        event.preventDefault()
        this.fragment.removeAttribute('src')
        this.fragment.setAttribute('data-loaded', '')
        oldDialog.replaceChildren(...newDialog.children)
        // Ensure the IDs are the same
        // FIXME: if we applied well known IDs we could delete the below code.
        for (const el of oldDialog.querySelectorAll('[data-close-dialog-id],[data-show-dialog-id]')) {
          if (el.getAttribute('data-close-dialog-id') === newDialog.id) {
            el.setAttribute('data-close-dialog-id', oldDialog.id)
          }
          if (el.getAttribute('data-show-dialog-id') === newDialog.id) {
            el.setAttribute('data-show-dialog-id', oldDialog.id)
          }
        }
      }
    })
  }
}


use strict";
(globalThis.webpackChunk = globalThis.webpackChunk || []).push([["app_assets_modules_github_behaviors_ajax-error_ts-app_assets_modules_github_behaviors_include-467754", "ui_packages_soft-navigate_soft-navigate_ts"], {
    50773: (e, t, r) => {
        r.d(t, {
            H: () => o,
            v: () => a
        });
        var n = r(59753);
        function a() {
            let e = document.getElementById("ajax-error-message");
            e && (e.hidden = !1)
        }
        function o() {
            let e = document.getElementById("ajax-error-message");
            e && (e.hidden = !0)
        }
        (0,
        n.on)("deprecatedAjaxError", "[data-remote]", function(e) {
            let {error: t, text: r} = e.detail;
            e.currentTarget === e.target && "abort" !== t && "canceled" !== t && (/<html/.test(r) ? (a(),
            e.stopImmediatePropagation()) : setTimeout(function() {
                e.defaultPrevented || a()
            }, 0))
        }),
        (0,
        n.on)("deprecatedAjaxSend", "[data-remote]", function() {
            o()
        }),
        (0,
        n.on)("click", ".js-ajax-error-dismiss", function() {
            o()
        })
    }
    ,
    88305: (e, t, r) => {
        r.d(t, {
            I: () => l,
            x: () => s
        });
        var n = r(36162)
          , a = r(36071)
          , o = r(59753)
          , i = r(7899);
        let d = new WeakMap;
        function l(e) {
            let t = e.closest(".js-render-needs-enrichment");
            t && (t.classList.remove("render-error"),
            d.get(t)?.setLoading(!1))
        }
        function s(e, t) {
            let r = e.closest(".js-render-needs-enrichment");
            return !!r && (r.classList.add("render-error"),
            d.get(r)?.setError(!0, t))
        }
        function c(e, t, r) {
            let a = r.identifier ?? ""
              , o = new URL(e,window.location.origin);
            for (let[e,r] of Object.entries(t))
                o.searchParams.append(e, `${r}`);
            return o.hash = a,
            (0,
            n.dy)`
    <div
      class="render-container color-bg-transparent js-render-target p-0"
      data-identity="${a}"
      data-host="${o.origin}"
      data-type="${r.type}"
    >
      <iframe
        title="File display"
        role="presentation"
        class="render-viewer"
        src="${String(o)}"
        name="${a}"
        data-content="${r.contentJson}"
        sandbox="allow-scripts allow-same-origin allow-top-navigation allow-popups"
      >
      </iframe>
    </div>
  `
        }
        (0,
        a.N7)(".js-render-needs-enrichment", {
            constructor: HTMLElement,
            initialize: function(e) {
                let t = {
                    color_mode: (0,
                    i.Fg)()
                }
                  , r = e.getAttribute("data-type")
                  , a = e.getAttribute("data-src")
                  , o = e.getAttribute("data-identity")
                  , l = e.getElementsByClassName("js-render-enrichment-target")[0]
                  , s = e.getElementsByClassName("js-render-enrichment-loader")[0]
                  , u = l.closest("details")
                  , m = document.createElement("div");
                m.classList.add("js-render-enrichment-fallback"),
                e.appendChild(m);
                let f = l.firstElementChild;
                m.appendChild(f);
                let h = {
                    setLoading(e) {
                        s.hidden = !e
                    },
                    setError: (e, t) => (h.setLoading(!1),
                    !1 !== e && (f.classList.toggle("render-plaintext-hidden", !e),
                    !!t && ((0,
                    n.sY)([t, f], m),
                    !0)))
                };
                d.set(e, h);
                let p = l.getAttribute("data-plain")
                  , g = l.getAttribute("data-json");
                if (null == g || null == p)
                    throw h.setError(!0, (0,
                    n.dy)`<p class="flash flash-error">Unable to render rich display</p>`),
                    Error(`Expected to see input data for type: ${r}`);
                let v = c(a, t, {
                    type: r,
                    identifier: o,
                    contentJson: g
                })
                  , y = c(a, t, {
                    type: r,
                    identifier: `${o}-fullscreen`,
                    contentJson: g
                })
                  , b = function(e, t, r) {
                    let a = (0,
                    n.dy)`<clipboard-copy
    aria-label="Copy ${r.type} code"
    .value=${e}
    class="btn my-2 js-clipboard-copy p-0 d-inline-flex tooltipped-no-delay"
    role="button"
    data-copy-feedback="Copied!"
    data-tooltip-direction="s"
  >
    <svg
      aria-hidden="true"
      height="16"
      viewBox="0 0 16 16"
      version="1.1"
      width="16"
      class="octicon octicon-copy js-clipboard-copy-icon m-2"
    >
      <path
        fill-rule="evenodd"
        d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 010 1.5h-1.5a.25.25 0 00-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 00.25-.25v-1.5a.75.75 0 011.5 0v1.5A1.75 1.75 0 019.25 16h-7.5A1.75 1.75 0 010 14.25v-7.5z"
      ></path>
      <path
        fill-rule="evenodd"
        d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0114.25 11h-7.5A1.75 1.75 0 015 9.25v-7.5zm1.75-.25a.25.25 0 00-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 00.25-.25v-7.5a.25.25 0 00-.25-.25h-7.5z"
      ></path>
    </svg>
    <svg
      aria-hidden="true"
      height="16"
      viewBox="0 0 16 16"
      version="1.1"
      width="16"
      class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none m-2"
    >
      <path
        fill-rule="evenodd"
        d="M13.78 4.22a.75.75 0 010 1.06l-7.25 7.25a.75.75 0 01-1.06 0L2.22 9.28a.75.75 0 011.06-1.06L6 10.94l6.72-6.72a.75.75 0 011.06 0z"
      ></path>
    </svg>
  </clipboard-copy>`
                      , o = (0,
                    n.dy)`
    <details class="details-reset details-overlay details-overlay-dark" style="display: contents">
      <summary
        role="button"
        aria-label="Open dialog"
        class="btn my-2 mr-2 p-0 d-inline-flex"
        aria-haspopup="dialog"
        @click="${t}"
      >
        <svg width="16" height="16" viewBox="0 0 16 16" fill="currentColor" class="octicon m-2">
          <path
            fill-rule="evenodd"
            d="M3.72 3.72a.75.75 0 011.06 1.06L2.56 7h10.88l-2.22-2.22a.75.75 0 011.06-1.06l3.5 3.5a.75.75 0 010 1.06l-3.5 3.5a.75.75 0 11-1.06-1.06l2.22-2.22H2.56l2.22 2.22a.75.75 0 11-1.06 1.06l-3.5-3.5a.75.75 0 010-1.06l3.5-3.5z"
          ></path>
        </svg>
      </summary>
      <details-dialog
        class="Box Box--overlay render-full-screen d-flex flex-column anim-fade-in fast"
        aria-label="${r.type} rendered container"
      >
        <div>
          <button
            aria-label="Close dialog"
            data-close-dialog=""
            type="button"
            data-view-component="true"
            class="Link--muted btn-link position-absolute render-full-screen-close"
          >
            <svg
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="currentColor"
              style="display:inline-block;vertical-align:text-bottom"
              class="octicon octicon-x"
            >
              <path
                fill-rule="evenodd"
                d="M5.72 5.72a.75.75 0 011.06 0L12 10.94l5.22-5.22a.75.75 0 111.06 1.06L13.06 12l5.22 5.22a.75.75 0 11-1.06 1.06L12 13.06l-5.22 5.22a.75.75 0 01-1.06-1.06L10.94 12 5.72 6.78a.75.75 0 010-1.06z"
              ></path>
            </svg>
          </button>
          <div class="Box-body border-0" role="presentation"></div>
        </div>
      </details-dialog>
    </details>
  `;
                    return (0,
                    n.dy)`<div class="position-absolute top-0 pr-2 right-0 d-flex flex-justify-end flex-items-center">
    ${o}${a}
  </div>`
                }(p, () => {
                    (0,
                    n.sY)(y, l.getElementsByClassName("Box-body")[0])
                }
                , {
                    type: r
                });
                u && !u.open ? u.ontoggle = () => {
                    u.open && ((0,
                    n.sY)([b, v], l),
                    u.ontoggle = null)
                }
                : (0,
                n.sY)([b, v], l)
            }
        }),
        (0,
        o.on)("preview:toggle:off", ".js-previewable-comment-form", function(e) {
            let t = e.currentTarget.querySelector(".js-render-needs-enrichment")
              , r = t?.querySelector(".js-render-enrichment-target");
            r && (r.textContent = "")
        }),
        (0,
        o.on)("preview:rendered", ".js-previewable-comment-form", function(e) {
            let t = e.currentTarget.querySelector(".js-render-needs-enrichment");
            t && d.get(t)?.setLoading(!1)
        })
    }
    ,
    64132: (e, t, r) => {
        r.d(t, {
            $: () => c,
            G: () => s
        });
        var n = r(8274)
          , a = r(36071)
          , o = r(59753);
        function i(e, t) {
            let r = e.currentTarget;
            if (r instanceof Element) {
                for (let e of r.querySelectorAll("[data-show-on-error]"))
                    e instanceof HTMLElement && (e.hidden = !t);
                for (let e of r.querySelectorAll("[data-hide-on-error]"))
                    e instanceof HTMLElement && (e.hidden = t)
            }
        }
        function d(e) {
            i(e, !1)
        }
        function l(e) {
            i(e, !0)
        }
        function s({currentTarget: e}) {
            e instanceof Element && c(e)
        }
        function c(e) {
            let t = e.closest("details");
            t && function(e) {
                let t = e.getAttribute("data-deferred-details-content-url");
                if (t) {
                    e.removeAttribute("data-deferred-details-content-url");
                    let r = e.querySelector("include-fragment, poll-include-fragment");
                    r && (r.src = t)
                }
            }(t)
        }
        (0,
        a.N7)("include-fragment, poll-include-fragment", {
            subscribe: e => (0,
            n.qC)((0,
            n.RB)(e, "error", l), (0,
            n.RB)(e, "loadstart", d))
        }),
        (0,
        o.on)("click", "include-fragment button[data-retry-button]", ({currentTarget: e}) => {
            e.closest("include-fragment").refetch()
        }
        )
    }
    ,
    90823: (e, t, r) => {
        r.d(t, {
            fK: () => v,
            j: () => g,
            ur: () => d
        });
        var n = r(36162)
          , a = r(88305)
          , o = r(36071)
          , i = r(31652);
        function d(e) {
            return !!e.querySelector('.js-render-target[data-type="ipynb"]')
        }
        let l = ["is-render-pending", "is-render-ready", "is-render-loading", "is-render-loaded"]
          , s = ["is-render-ready", "is-render-loading", "is-render-loaded", "is-render-failed", "is-render-failed-fatally"]
          , c = new WeakMap;
        function u(e) {
            let t = c.get(e);
            null != t && (t.load = t.hello = null,
            t.helloTimer && (clearTimeout(t.helloTimer),
            t.helloTimer = null),
            t.loadTimer && (clearTimeout(t.loadTimer),
            t.loadTimer = null))
        }
        function m(e, t="") {
            e.classList.remove(...l),
            e.classList.add("is-render-failed");
            let r = function(e) {
                let t = (0,
                n.dy)`<p>Unable to render rich display</p>`;
                if ("" !== e) {
                    let r = e.split("\n");
                    t = (0,
                    n.dy)`<p><b>Unable to render rich display</b></p>
      <p>${r.map(e => (0,
                    n.dy)`${e}<br />`)}</p>`
                }
                return (0,
                n.dy)`<div class="flash flash-error">${t}</div>`
            }(t);
            !1 === (0,
            a.x)(e, r) && function(e, t) {
                let r = e.querySelector(".render-viewer-error");
                r && (r.remove(),
                e.classList.remove("render-container"),
                (0,
                n.sY)(t, e))
            }(e, r),
            u(e)
        }
        function f(e, t=!1) {
            !(!(0,
            i.Z)(e) || e.classList.contains("is-render-ready") || e.classList.contains("is-render-failed") || e.classList.contains("is-render-failed-fatally")) && (!t || c.get(e)?.hello) && m(e)
        }
        function h(e, t) {
            return !!e && !!e.postMessage && (e.postMessage(JSON.stringify(t), "*"),
            !0)
        }
        function p(e) {
            return t => {
                if (!t.querySelector(".js-render-target"))
                    return;
                let r = t.querySelector("iframe")
                  , n = r?.contentWindow;
                if (n)
                    return e(n)
            }
        }
        (0,
        o.N7)(".js-render-target", function(e) {
            e.classList.remove(...s),
            e.style.height = "auto",
            !c.get(e)?.load && (u(e),
            c.get(e) || (c.set(e, {
                load: Date.now(),
                hello: null,
                helloTimer: window.setTimeout(f, 1e4, e, !0),
                loadTimer: window.setTimeout(f, 45e3, e)
            }),
            e.classList.add("is-render-automatic", "is-render-requested")))
        }),
        window.addEventListener("message", function(e) {
            let t = e.data;
            if (!t)
                return;
            if ("string" == typeof t)
                try {
                    t = JSON.parse(t)
                } catch {
                    return
                }
            if ("object" != typeof t && void 0 != t || "render" !== t.type || "string" != typeof t.identity)
                return;
            let r = t.identity;
            if ("string" != typeof t.body)
                return;
            let n = t.body
              , o = function(e, t) {
                let r = e.querySelector(`.js-render-target[data-identity="${t}"]`);
                return r && r instanceof HTMLElement ? r : null
            }(document, r);
            if (!o || e.origin !== o.getAttribute("data-host"))
                return;
            let i = null != t.payload ? t.payload : void 0
              , d = o.querySelector("iframe")
              , s = d?.contentWindow;
            switch (n) {
            case "hello":
                if ((c.get(o) || {
                    untimed: !0
                }).hello = Date.now(),
                !s)
                    return;
                h(s, {
                    type: "render:cmd",
                    body: {
                        cmd: "ack",
                        ack: !0
                    }
                }),
                h(s, {
                    type: "render:cmd",
                    body: {
                        cmd: "branding",
                        branding: !1
                    }
                });
                break;
            case "error":
                m(o, i?.error);
                break;
            case "error:fatal":
                m(o, i?.error),
                o.classList.add("is-render-failed-fatal");
                break;
            case "error:invalid":
                m(o, i?.error),
                o.classList.add("is-render-failed-invalid");
                break;
            case "loading":
                o.classList.remove(...l),
                o.classList.add("is-render-loading");
                break;
            case "loaded":
                o.classList.remove(...l),
                o.classList.add("is-render-loaded");
                break;
            case "ready":
                (0,
                a.I)(o),
                o.classList.remove(...l),
                o.classList.add("is-render-ready"),
                i && "number" == typeof i.height && (o.style.height = `${i.height}px`,
                "" !== location.hash && window.dispatchEvent(new HashChangeEvent("hashchange"))),
                i?.ack === !0 && window.requestAnimationFrame( () => {
                    setTimeout( () => {
                        h(s, {
                            type: "render:cmd",
                            body: {
                                cmd: "code_rendering_service:ready:ack",
                                "code_rendering_service:ready:ack": {}
                            }
                        })
                    }
                    , 0)
                }
                );
                break;
            case "resize":
                i && "number" == typeof i.height && (o.style.height = `${i.height}px`);
                break;
            case "code_rendering_service:container:get_size":
                h(s, {
                    type: "render:cmd",
                    body: {
                        cmd: "code_rendering_service:container:size",
                        "code_rendering_service:container:size": {
                            width: o?.getBoundingClientRect().width
                        }
                    }
                });
                break;
            case "code_rendering_service:markdown:get_data":
                if (!s)
                    return;
                !function() {
                    let e;
                    let t = d?.getAttribute("data-content") ?? "";
                    try {
                        e = JSON.parse(t)?.data
                    } catch {
                        e = null
                    }
                    e && h(s, {
                        type: "render:cmd",
                        body: {
                            cmd: "code_rendering_service:data:ready",
                            "code_rendering_service:data:ready": {
                                data: e,
                                width: o?.getBoundingClientRect().width
                            }
                        }
                    })
                }()
            }
        });
        let g = p(e => h(e, {
            type: "render:cmd",
            body: {
                cmd: "code_rendering_service:behaviour:expand_all"
            }
        }))
          , v = p(e => h(e, {
            type: "render:cmd",
            body: {
                cmd: "code_rendering_service:behaviour:collapse_all"
            }
        }))
    }
    ,
    7899: (e, t, r) => {
        r.d(t, {
            Fg: () => u,
            I3: () => i,
            h5: () => l,
            on: () => s,
            yn: () => c
        });
        var n = r(37083)
          , a = r(90532);
        function o() {
            (0,
            a.d8)("preferred_color_mode", i())
        }
        function i() {
            return d("dark") ? "dark" : d("light") ? "light" : void 0
        }
        function d(e) {
            return window.matchMedia && window.matchMedia(`(prefers-color-scheme: ${e})`).matches
        }
        function l(e) {
            let t = document.querySelector("html[data-color-mode]");
            t && t.setAttribute("data-color-mode", e)
        }
        function s(e, t) {
            let r = document.querySelector("html[data-color-mode]");
            r && r.setAttribute(`data-${t}-theme`, e)
        }
        function c(e) {
            let t = document.querySelector("html[data-color-mode]");
            if (t)
                return t.getAttribute(`data-${e}-theme`)
        }
        function u(e="light") {
            let t = function() {
                let e = document.querySelector("html[data-color-mode]");
                if (e)
                    return e.getAttribute("data-color-mode")
            }();
            return ("auto" === t ? i() : t) ?? e
        }
        (async () => {
            if (await n.x,
            o(),
            window.matchMedia) {
                let e = window.matchMedia("(prefers-color-scheme: dark)");
                e?.addEventListener ? e.addEventListener("change", o) : e.addListener(o)
            }
        }
        )()
    }
    ,
    4034: (e, t, r) => {
        r.d(t, {
            N: () => i,
            x: () => d
        });
        var n = r(930)
          , a = r(14203)
          , o = r(28907);
        function i(e, t) {
            (0,
            a.cr)("primer_live_region_element") && t?.element === void 0 ? (0,
            o.Nz)(e, {
                politeness: t?.assertive ? "assertive" : "polite"
            }) : d((e.getAttribute("aria-label") || e.innerText || "").trim(), t)
        }
        function d(e, t) {
            let {assertive: r, element: i} = t ?? {};
            (0,
            a.cr)("primer_live_region_element") && void 0 === i ? (0,
            o.xQ)(e, {
                politeness: r ? "assertive" : "polite"
            }) : function(e, t, r) {
                let a = r ?? n.n4?.querySelector(t ? "#js-global-screen-reader-notice-assertive" : "#js-global-screen-reader-notice");
                a && (a.textContent === e ? a.textContent = `${e}\u00A0` : a.textContent = e)
            }(e, r, i)
        }
    }
    ,
    90532: (e, t, r) => {
        function n(e) {
            return a(e)[0]
        }
        function a(e) {
            let t = [];
            for (let r of function() {
                try {
                    return document.cookie.split(";")
                } catch {
                    return []
                }
            }()) {
                let[n,a] = r.trim().split("=");
                e === n && void 0 !== a && t.push({
                    key: n,
                    value: a
                })
            }
            return t
        }
        function o(e, t, r=null, n=!1, a="lax") {
            let o = document.domain;
            if (null == o)
                throw Error("Unable to get document domain");
            o.endsWith(".github.com") && (o = "github.com");
            let i = "https:" === location.protocol ? "; secure" : ""
              , d = r ? `; expires=${r}` : "";
            !1 === n && (o = `.${o}`);
            try {
                document.cookie = `${e}=${t}; path=/; domain=${o}${d}${i}; samesite=${a}`
            } catch {}
        }
        function i(e, t=!1) {
            let r = document.domain;
            if (null == r)
                throw Error("Unable to get document domain");
            r.endsWith(".github.com") && (r = "github.com");
            let n = new Date(new Date().getTime() - 1).toUTCString()
              , a = "https:" === location.protocol ? "; secure" : ""
              , o = `; expires=${n}`;
            !1 === t && (r = `.${r}`);
            try {
                document.cookie = `${e}=''; path=/; domain=${r}${o}${a}`
            } catch {}
        }
        r.d(t, {
            $1: () => a,
            d8: () => o,
            ej: () => n,
            kT: () => i
        })
    }
    ,
    29669: (e, t, r) => {
        r.d(t, {
            $g: () => SoftNavSuccessEvent,
            OV: () => SoftNavStartEvent,
            QW: () => SoftNavErrorEvent,
            Xr: () => SoftNavEndEvent
        });
        var n = r(21118);
        let a = class SoftNavEvent extends Event {
            constructor(e, t) {
                super(t),
                this.mechanism = e
            }
        }
        ;
        let SoftNavStartEvent = class SoftNavStartEvent extends a {
            constructor(e) {
                super(e, n.Q.START)
            }
        }
        ;
        let SoftNavSuccessEvent = class SoftNavSuccessEvent extends a {
            constructor(e, t) {
                super(e, n.Q.SUCCESS),
                this.visitCount = t
            }
        }
        ;
        let SoftNavErrorEvent = class SoftNavErrorEvent extends a {
            constructor(e, t) {
                super(e, n.Q.ERROR),
                this.error = t
            }
        }
        ;
        let SoftNavEndEvent = class SoftNavEndEvent extends a {
            constructor(e) {
                super(e, n.Q.END)
            }
        }
    }
    ,
    56163: (e, t, r) => {
        r.d(t, {
            BT: () => c,
            FP: () => m,
            LD: () => s,
            TL: () => f,
            Yl: () => l,
            r_: () => u,
            u5: () => h
        });
        var n = r(21118)
          , a = r(29669)
          , o = r(15815)
          , i = r(14220);
        let d = 0;
        function l() {
            d = 0,
            document.dispatchEvent(new Event(n.Q.INITIAL)),
            (0,
            i.XL)()
        }
        function s(e) {
            (0,
            i.sj)() || (document.dispatchEvent(new Event(n.Q.PROGRESS_BAR.START)),
            document.dispatchEvent(new a.OV(e)),
            (0,
            i.U6)(e),
            (0,
            i.J$)(),
            (0,
            i.Nt)(),
            (0,
            o.hY)())
        }
        function c(e={}) {
            g(e) && (d += 1,
            document.dispatchEvent(new a.$g((0,
            i.Gj)(),d)),
            m(e))
        }
        function u(e={}) {
            if (!g(e))
                return;
            d = 0;
            let t = (0,
            i.Wl)() || i.jN;
            document.dispatchEvent(new a.QW((0,
            i.Gj)(),t)),
            p(),
            (0,
            o.t3)(t),
            (0,
            i.XL)()
        }
        function m(e={}) {
            if (!g(e))
                return;
            let t = (0,
            i.Gj)();
            p(),
            document.dispatchEvent(new a.Xr(t)),
            (0,
            i.pS)(),
            (0,
            i.vu)(t)
        }
        function f(e={}) {
            g(e) && ((0,
            o.mr)(),
            document.dispatchEvent(new Event(n.Q.RENDER)))
        }
        function h() {
            document.dispatchEvent(new Event(n.Q.FRAME_UPDATE))
        }
        function p() {
            document.dispatchEvent(new Event(n.Q.PROGRESS_BAR.END))
        }
        function g({skipIfGoingToReactApp: e, allowedMechanisms: t=[]}={}) {
            return (0,
            i.sj)() && (0 === t.length || t.includes((0,
            i.Gj)())) && (!e || !(0,
            i.Nb)())
        }
    }
    ,
    15815: (e, t, r) => {
        r.d(t, {
            CF: () => i,
            aq: () => o,
            hY: () => d,
            mr: () => s,
            t3: () => l
        });
        var n = r(19963)
          , a = r(14220);
        let o = "stats:soft-nav-duration"
          , i = {
            turbo: "TURBO",
            react: "REACT",
            "turbo.frame": "FRAME",
            ui: "UI",
            hard: "HARD"
        };
        function d() {
            performance.clearResourceTimings(),
            performance.mark(o)
        }
        function l(e) {
            (0,
            n.b)({
                turboFailureReason: e,
                turboStartUrl: (0,
                a.wP)(),
                turboEndUrl: window.location.href
            })
        }
        function s() {
            let e = function() {
                if (0 === performance.getEntriesByName(o).length)
                    return null;
                performance.measure(o, o);
                let e = performance.getEntriesByName(o).pop();
                return e ? e.duration : null
            }();
            if (!e)
                return;
            let t = i[(0,
            a.Gj)()]
              , r = Math.round(e);
            t === i.react && document.dispatchEvent(new CustomEvent("staffbar-update",{
                detail: {
                    duration: r
                }
            })),
            (0,
            n.b)({
                requestUrl: window.location.href,
                softNavigationTiming: {
                    mechanism: t,
                    destination: (0,
                    a.Nb)() || "rails",
                    duration: r,
                    initiator: (0,
                    a.CI)() || "rails"
                }
            })
        }
    }
    ,
    70229: (e, t, r) => {
        r.d(t, {
            softNavigate: () => o
        });
        var n = r(56163)
          , a = r(67852);
        function o(e, t) {
            (0,
            n.LD)("turbo"),
            (0,
            a.Vn)(e, {
                ...t
            })
        }
    }
}]);
//# sourceMappingURL=app_assets_modules_github_behaviors_ajax-error_ts-app_assets_modules_github_behaviors_include-467754-553e19887e9d.js.map

"use strict";
(globalThis.webpackChunk = globalThis.webpackChunk || []).push([["app_assets_modules_github_behaviors_commenting_edit_ts-app_assets_modules_github_behaviors_ht-83c235"], {
    22327: (e, t, s) => {
        s.d(t, {
            z: () => y
        });
        var n = s(59753)
          , i = s(55129)
          , o = s(14936)
          , r = s(95059)
          , l = s(8274)
          , a = s(27103)
          , m = s(36071)
          , c = s(65935)
          , u = s(55878);
        let d = [];
        function f(e) {
            e.querySelector(".js-write-tab").click();
            let t = e.querySelector(".js-comment-field");
            t.focus(),
            (0,
            n.f)(t, "change")
        }
        function j(e) {
            return e.querySelector(".js-comment-edit-form-deferred-include-fragment")
        }
        function g(e) {
            j(e)?.setAttribute("loading", "eager")
        }
        function y(e) {
            let t = e.currentTarget.closest("form")
              , s = e.currentTarget.getAttribute("data-confirm-text");
            if ((0,
            a.T)(t) && !confirm(s))
                return !1;
            for (let e of t.querySelectorAll("input, textarea"))
                e.value = e.defaultValue,
                e.classList.contains("session-resumable-canceled") && (e.classList.add("js-session-resumable"),
                e.classList.remove("session-resumable-canceled"));
            let n = e.currentTarget.closest(".js-comment");
            return n && n.classList.remove("is-comment-editing"),
            !0
        }
        function p(e) {
            let t = e.querySelector("ol");
            if (t)
                for (let e of (t.textContent = "",
                d.map(e => {
                    let t = document.createElement("li");
                    return t.textContent = e,
                    t
                }
                )))
                    t.appendChild(e);
            e.hidden = !1
        }
        function h(e, t) {
            let s = e.querySelector(".js-comment-show-on-error");
            s && (s.hidden = !t);
            let n = e.querySelector(".js-comment-hide-on-error");
            n && (n.hidden = t)
        }
        (0,
        m.N7)(".js-comment-header-actions-deferred-include-fragment", {
            subscribe: e => (0,
            l.RB)(e, "loadstart", () => {
                g(e.closest(".js-comment"))
            }
            , {
                capture: !1,
                once: !0
            })
        }),
        (0,
        m.N7)(".js-comment .contains-task-list", {
            add: e => {
                g(e.closest(".js-comment"))
            }
        }),
        (0,
        n.on)("click", ".js-comment-edit-button", function(e) {
            let t = e.currentTarget.closest(".js-comment");
            t.classList.add("is-comment-editing");
            let s = j(t);
            s ? s.addEventListener("include-fragment-replaced", () => f(t), {
                once: !0
            }) : f(t);
            let n = e.currentTarget.closest(".js-dropdown-details");
            n && n.removeAttribute("open")
        }),
        (0,
        n.on)("click", ".js-comment-hide-button", function(e) {
            let t = e.currentTarget.closest(".js-comment");
            h(t, !1);
            let s = t.querySelector(".js-minimize-comment");
            s && s.classList.remove("d-none");
            let n = e.currentTarget.closest(".js-dropdown-details");
            n && n.removeAttribute("open")
        }),
        (0,
        n.on)("click", ".js-comment-hide-minimize-form", function(e) {
            e.currentTarget.closest(".js-minimize-comment").classList.add("d-none")
        }),
        (0,
        n.on)("click", ".js-comment-cancel-button", y),
        (0,
        n.on)("click", ".js-cancel-issue-edit", function(e) {
            e.currentTarget.closest(".js-details-container").querySelector(".js-comment-form-error").hidden = !0
        }),
        (0,
        c.AC)(".js-comment-delete, .js-comment .js-comment-update, .js-issue-update, .js-comment-minimize, .js-comment-unminimize", function(e, t, s) {
            let n = e.closest(".js-comment");
            n.classList.add("is-comment-loading");
            let i = n.getAttribute("data-body-version");
            i && s.headers.set("X-Body-Version", i)
        }),
        (0,
        c.AC)(".js-comment .js-comment-update", async function(e, t) {
            let s;
            let n = e.closest(".js-comment")
              , o = n.querySelector(".js-comment-update-error")
              , l = n.querySelector(".js-comment-body-error");
            o instanceof HTMLElement && (o.hidden = !0),
            l instanceof HTMLElement && (l.hidden = !0),
            d = [],
            e.classList.add("is-dirty");
            try {
                s = await t.json()
            } catch (e) {
                if (422 === e.response.status) {
                    let t = JSON.parse(e.response.text);
                    if (t.errors) {
                        o instanceof HTMLElement && (o.textContent = `There was an error posting your comment: ${t.errors.join(", ")}`,
                        o.hidden = !1);
                        return
                    }
                } else
                    throw e
            } finally {
                e.classList.remove("is-dirty")
            }
            if (!s)
                return;
            let m = s.json;
            m.errors && m.errors.length > 0 && (d = m.errors,
            p(l));
            let c = n.querySelector(".js-comment-body")
              , u = null != c && "async" === e.getAttribute("data-submitting-tracking-block-update") && (0,
            a.M)(c, !0, !0);
            if (c && m.body && !u && (0,
            i.jI)(c, m.body, {
                keepInputValues: !0
            }),
            null != m.source) {
                let e = n.querySelector(".js-comment-field");
                if (e && (e.defaultValue = m.source,
                u || (e.value = m.source)),
                m.default_merge_commit_message) {
                    if (document.querySelector(".js-merge-pr.is-merging")) {
                        let e = document.querySelector(".js-merge-pull-request textarea");
                        e instanceof HTMLTextAreaElement && e.value === e.defaultValue && (e.value = e.defaultValue = m.default_merge_commit_message)
                    }
                    if (m.default_squash_commit_message && document.querySelector(".js-merge-pr.is-squashing")) {
                        let e = document.querySelector(".js-merge-pull-request textarea");
                        e instanceof HTMLTextAreaElement && e.value === e.defaultValue && (e.value = e.defaultValue = m.default_squash_commit_message)
                    }
                }
                document.querySelector(".js-merge-box-button-merge")?.setAttribute("data-input-message-value", m.default_merge_commit_message),
                document.querySelector(".js-merge-box-button-squash")?.setAttribute("data-input-message-value", m.default_squash_commit_message)
            }
            n.setAttribute("data-body-version", m.newBodyVersion);
            let f = n.querySelector(".js-body-version");
            f instanceof HTMLInputElement && (f.value = m.newBodyVersion);
            let j = n.querySelector(".js-discussion-poll");
            for (let e of (j && m.poll && (j.innerHTML = m.poll),
            n.querySelectorAll("input, textarea")))
                e.defaultValue = e.value;
            n.classList.remove("is-comment-stale"),
            e.hasAttribute("data-submitting-tracking-block-update") || n.classList.remove("is-comment-editing");
            let g = n.querySelector(".js-comment-edit-history");
            if (g) {
                let e = await (0,
                r.a_)(document, m.editUrl);
                (0,
                i.nn)(g, e)
            }
        }),
        (0,
        m.N7)(".js-comment-body-error", {
            add: e => {
                d && d.length > 0 && p(e)
            }
        }),
        (0,
        c.AC)(".js-comment .js-comment-delete, .js-comment .js-comment-update, .js-comment-minimize, .js-comment-unminimize", async function(e, t) {
            let s;
            let n = e.closest(".js-comment");
            try {
                await t.text()
            } catch (e) {
                if (422 === (s = e).response.status) {
                    let e;
                    try {
                        e = JSON.parse(s.response.text)
                    } catch (e) {}
                    e && e.stale && n.classList.add("is-comment-stale")
                } else
                    throw s
            } finally {
                e.dispatchEvent(new CustomEvent("submit:complete",{
                    bubbles: !0,
                    detail: {
                        error: s
                    }
                }))
            }
            n.classList.remove("is-comment-loading")
        }),
        (0,
        c.AC)(".js-timeline-comment-unminimize, .js-timeline-comment-minimize", async function(e, t) {
            let s = e.closest(".js-minimize-container");
            try {
                let e = await t.html();
                s.replaceWith(e.html)
            } catch (e) {
                h(s, !0)
            }
        }),
        (0,
        c.AC)(".js-discussion-comment-unminimize, .js-discussion-comment-minimize", async function(e, t) {
            let s = e.closest(".js-discussion-comment")
              , n = s.querySelector(".js-discussion-comment-error");
            n && (n.hidden = !0);
            try {
                let e = await t.html();
                s.replaceWith(e.html)
            } catch (e) {
                if (e.response.status >= 400 && e.response.status < 500) {
                    if (e.response.html) {
                        let t = e.response.html.querySelector(".js-discussion-comment").getAttribute("data-error");
                        n instanceof HTMLElement && (n.textContent = t,
                        n.hidden = !1)
                    }
                } else
                    throw e
            }
        }),
        (0,
        c.AC)(".js-comment-delete", async function(e, t) {
            await t.json();
            let s = e.closest(".js-comment-container") || e.closest(".js-line-comments");
            s && 1 !== s.querySelectorAll(".js-comment").length && (s = e.closest(".js-comment"));
            let n = s?.closest(".js-comment-container") || s?.closest(".js-line-comments");
            if (s?.remove(),
            n && 1 === n.querySelectorAll(".js-comment").length)
                for (let e of n.querySelectorAll(".js-delete-on-last-reply-deleted"))
                    e.remove()
        }),
        (0,
        c.AC)(".js-issue-update", async function(e, t) {
            let s;
            let n = e.closest(".js-details-container")
              , i = n.querySelector(".js-comment-form-error");
            try {
                s = await t.json()
            } catch (e) {
                i.textContent = e.response?.json?.errors?.[0] || "Something went wrong. Please try again.",
                i.hidden = !1
            }
            if (!s)
                return;
            n.classList.remove("open"),
            i.hidden = !0;
            let o = s.json;
            if (null != o.issue_title) {
                n.querySelector(".js-issue-title").textContent = o.issue_title;
                let e = n.closest(".js-issues-results");
                if (e) {
                    if (e.querySelector(".js-merge-pr.is-merging")) {
                        let t = e.querySelector(".js-merge-pull-request .js-merge-title");
                        t instanceof HTMLInputElement && t.value === t.defaultValue && (t.value = t.defaultValue = o.default_merge_commit_title)
                    } else if (e.querySelector(".js-merge-pr.is-squashing")) {
                        let t = e.querySelector(".js-merge-pull-request .js-merge-title");
                        t instanceof HTMLInputElement && t.value === t.defaultValue && (t.value = t.defaultValue = o.default_squash_commit_title)
                    }
                    let t = e.querySelector("button[value=merge]");
                    t && t.setAttribute("data-input-title-value", o.default_merge_commit_title);
                    let s = e.querySelector("button[value=squash]");
                    s && s.setAttribute("data-input-title-value", o.default_squash_commit_title)
                }
            }
            for (let t of (document.title = o.page_title,
            e.elements))
                (t instanceof HTMLInputElement || t instanceof HTMLTextAreaElement) && (t.defaultValue = t.value)
        }),
        (0,
        c.AC)(".js-comment-minimize", async function(e, t) {
            await t.json();
            let s = e.closest(".js-comment")
              , n = s.querySelector(".js-minimize-comment");
            if (n && n.classList.contains("js-update-minimized-content")) {
                let t = e.querySelector("input[type=submit], button[type=submit]");
                t && t.classList.add("disabled");
                let n = s.closest(".js-comment-container");
                n && await (0,
                u.x0)(n)
            } else {
                n && n.classList.add("d-none");
                let t = e.closest(".unminimized-comment");
                t.classList.add("d-none"),
                t.classList.remove("js-comment");
                let s = e.closest(".js-minimizable-comment-group").querySelector(".minimized-comment");
                s && s.classList.remove("d-none"),
                s && s.classList.add("js-comment")
            }
        }),
        (0,
        c.AC)(".js-comment-unminimize", async function(e, t) {
            await t.json();
            let s = e.closest(".js-minimizable-comment-group")
              , n = s.querySelector(".unminimized-comment")
              , i = s.querySelector(".minimized-comment");
            if (n)
                n.classList.remove("d-none"),
                n.classList.add("js-comment"),
                i && i.classList.add("d-none"),
                i && i.classList.remove("js-comment");
            else {
                if (i) {
                    let e = i.querySelector(".timeline-comment-actions");
                    e && e.classList.add("d-none"),
                    i.classList.remove("js-comment")
                }
                let e = s.closest(".js-comment-container");
                await (0,
                u.x0)(e)
            }
        }),
        (0,
        n.on)("details-menu-select", ".js-comment-edit-history-menu", e => {
            let t = e.detail.relatedTarget.getAttribute("data-edit-history-url");
            if (!t)
                return;
            e.preventDefault();
            let s = (0,
            r.a_)(document, t);
            (0,
            o.W)({
                content: s,
                dialogClass: "Box-overlay--wide overflow-visible",
                errorMessage: "Couldn't display edit history diff"
            })
        }
        , {
            capture: !0
        })
    }
    ,
    22455: (e, t, s) => {
        s.d(t, {
            G: () => a
        });
        var n = s(73444)
          , i = s(36071)
          , o = s(59753);
        function r(e) {
            let t = e.getAttribute("data-required-value")
              , s = e.getAttribute("data-required-value-prefix");
            if (e.value === t)
                e.setCustomValidity("");
            else {
                let n = t;
                s && (n = s + n),
                e.setCustomValidity(n)
            }
        }
        (0,
        n.q6)("[data-required-value]", function(e) {
            r(e.currentTarget)
        }),
        (0,
        o.on)("change", "[data-required-value]", function(e) {
            let t = e.currentTarget;
            r(t),
            a(t.form)
        }),
        (0,
        n.q6)("[data-required-trimmed]", function(e) {
            let t = e.currentTarget;
            "" === t.value.trim() ? t.setCustomValidity(t.getAttribute("data-required-trimmed")) : t.setCustomValidity("")
        }),
        (0,
        o.on)("change", "[data-required-trimmed]", function(e) {
            let t = e.currentTarget;
            "" === t.value.trim() ? t.setCustomValidity(t.getAttribute("data-required-trimmed")) : t.setCustomValidity(""),
            a(t.form)
        }),
        (0,
        n.ZG)("input[pattern],input[required],textarea[required],input[data-required-change],textarea[data-required-change],input[data-required-value],textarea[data-required-value]", e => {
            let t = e.checkValidity();
            function s() {
                let s = e.checkValidity();
                s !== t && e.form && a(e.form),
                t = s
            }
            e.addEventListener("input", s),
            e.addEventListener("blur", function t() {
                e.removeEventListener("input", s),
                e.removeEventListener("blur", t)
            })
        }
        );
        let l = new WeakMap;
        function a(e) {
            let t = e.checkValidity();
            for (let s of e.querySelectorAll("button[data-disable-invalid]"))
                s.disabled = !t
        }
        (0,
        i.N7)("button[data-disable-invalid]", {
            constructor: HTMLButtonElement,
            initialize(e) {
                let t = e.form;
                t && (l.get(t) || (t.addEventListener("change", () => a(t)),
                l.set(t, !0)),
                e.disabled = !t.checkValidity())
            }
        }),
        (0,
        i.N7)("input[data-required-change], textarea[data-required-change]", function(e) {
            let t = "radio" === e.type && e.form ? e.form.elements.namedItem(e.name).value : null;
            function s(s) {
                let n = e.form;
                if (s && "radio" === e.type && n && t)
                    for (let s of n.elements.namedItem(e.name))
                        s instanceof HTMLInputElement && s.setCustomValidity(e.value === t ? "unchanged" : "");
                else
                    e.setCustomValidity(e.value === (t || e.defaultValue) ? "unchanged" : "")
            }
            e.addEventListener("input", s),
            e.addEventListener("change", s),
            s(),
            e.form && a(e.form)
        }),
        document.addEventListener("reset", function(e) {
            if (e.target instanceof HTMLFormElement) {
                let t = e.target;
                setTimeout( () => a(t))
            }
        })
    }
}]);
//# sourceMappingURL=app_assets_modules_github_behaviors_commenting_edit_ts-app_assets_modules_github_behaviors_ht-83c235-822446d7f34e.js.map
